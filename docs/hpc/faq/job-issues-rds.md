# Jobs causing issues with the RDS

Quite often, we come across users whose jobs slowdown the RDS, increase significant load on the RDS or degrade its performance in any other way. In some cases, the RCS team has to quickly figure out the job causing such issues. When the team finds such a user, their job has to be killed or we have to hold it for further checks so that other users are not affected. The RCS team may also send an email to the user asking them to review their workflows and talk to the RCS team for any help.

In the recent past, we had quite a few such occasions. In almost all cases, the reason was reading the same input file by multiple jobs, subjobs, processes etc. We would also like to categorically point out that any code doing Parallel I/O using MPI or any other similar libraries cannot cause such issues as they are designed to allow multiple processes to read the same file.

In order to understand how reading from same file can cause issues, take a look at this example script:

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=50gb
#PBS -l walltime=00:30:00
#PBS -N my_cpp_array_job
#PBS -t 1-10
 
# Set the working directory
cd $PBS_O_WORKDIR
 
# Load any necessary modules for your C++ program
module load tools/prod
module load GCC/11.2.0
 
# Define the input file location
input_file="/home/input_file_location/my_input_file.txt"
 
# Define the output directory
output_dir="/home/output"
 
# Create the output directory if it does not exist
mkdir -p $output_dir
 
# Define the output file for each subjob using PBS_ARRAY_INDEX
output_file="${output_dir}/my_out_${PBS_ARRAY_INDEX}.txt"
 
# Run your C++ program with the specified input and output files
./your_cpp_program -i $input_file -o $output_file
 
# Print a message indicating the subjob has completed
echo "Subjob ${PBS_ARRAY_INDEX} has completed."
```

In the above script, we are running an array job (For more details, please see [Array Jobs](../queues/array-jobs.md) and specifically [Strategies for Writing Array Job Submission Scripts](../queues/array-jobs.md#strategies-for-writing-array-jobs)) which runs 10 sub-jobs each requiring 4 cores. Each sub-job is reading from a single file /home/input_file_location/my_input_file.txt and writing to a separate file as can be seen in `output_file="${output_dir}/my_out_${PBS_ARRAY_INDEX}.txt"`.

Now depending on the load on the cluster, it may happen that some of these 10 jobs start roughly at the same time. Assume that 5 jobs start at the same time. For simplicity, further assume that only the master thread/process is reading the file while others are waiting. This means out of 4 processes (ppn value), only 1 of them reads the entire file. Since 5 jobs started at the same time, it cause problems to OS to decide which of these should be allowed to read the file first. This finally results in file locking (more details can be found in [https://www.egnyte.com/guides/file-sharing/file-locking](https://www.egnyte.com/guides/file-sharing/file-locking)).

Please note that not just the reading but such an attempt to write the same file can lead to worse results (details skipped here as they are not directly relevant).

## How to resolve this file locking issue
There are multiple steps that you can use in your job script that can reduce or completely eliminate file locking issues (and high load on the RDS that we talked about earlier).

* [Use Staging](#use-staging)
* [Use a separate location or unique copy of input/output files for your sub-jobs](#use-a-separate-location-or-unique-copy-of-inputoutput-files-for-your-sub-jobs)

### Use Staging

One of the causes of high load on the RDS is the fact the job under consideration is I/O intensive i.e. there is lots of reads and/or writes to the disk. Depending on the input file, frequency of reads, number of times you write the data and how much you write the date, it may be an excellent idea to use Staging in your applications.

A staging area is a temporary fast storage between the compute hardware (processor/cores etc.) and the hard disk. It is slower than RAM but faster than the hard disk itself and therefore applications relying on high I/O will benefit from Staging in general. However, for jobs which are not I/O intensive, it will not make much difference. Please keep in mind that the maximum filesize on one node for Staging is approximately 200 GB. If your workflow involves reading and writing files of size much higher than this, then Staging will not help and you should not use TMPDIR for your job. 

You can find more details about staging on using TMPDIR on the [Best Practice page](../best-practice.md#stage-via-tmpdir).

### Use a separate location or unique copy of input/output files for your sub-jobs.

As you can possibly imagine, one of the root causes of file locking was due to the fact that multiple sub-jobs were trying to read from the same file. Thus, it makes sense to provide input files at different locations for each sub job. Quite often, this step is one of the most important and eliminates the issue completely.

For users whose input file size is small, it is easy to make copies (let us say 10) of these files at unique locations so that each sub-job reads from its unique location. However, for users whose input  files are very large, this step may not be possible. In that case, we suggest you to limit the number of sub-jobs that can be launched simultaneously or make use of dependency in your job scripts as described in Job Dependencies.

We hope that you find this information useful and with these steps, you should be able to eliminate or reduce the RDS issues that your job might have caused.
