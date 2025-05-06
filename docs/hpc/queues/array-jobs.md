# Array Jobs

!!! info

    This page **has** been rewritten for CX3 Phase 2.

Often you may find that you want to run a large number of very similar jobs. Rather than directly submitting each task as a separate job, you should consider using an array job.

## What to consider before using an array job

There are a number of points to consider before you start running array jobs. If you have any concerns regarding these, please contact us before submitting array jobs.

### File system load

The RCS often observe cases where array jobs generate problematic load on the RDS file system. This can be due to either:

* A single sub-job generating a lot of file system load. When many are run at the same time, the problem is compounded to the extent that the file system cannot keep up.
* The application being run is reading/writing to the same set of files on the RDS and the application itself is not designed to do this.

### Job Scheduler Limits

There are controls in place on the job scheduler that limit the number of array sub jobs that may be run at any one time. These limits are in place to ensure fair usage of the system for all users (otherwise array jobs could dominate the clusters) and also to limit the load on the RCS facilities in the event that a particular workflow generates problematic load on either the network or filesystem.

## Submitting array jobs

Having created the jobscript as you would do normally, add in the additional directive:

```bash
#PBS -J 1-N
```

where `N` is the number of copies of the job you want to run. You may then `qsub` the jobscript once, and the system will run N instances of it. Each of these sub-jobs run independently of all of the others, and are identical except for the value of the environment variable `PBS_ARRAY_INDEX`. This will contain a unique value in the range 1-N, allowing you to select the particular input for that sub-job. The range does not need to start at 1, but there is a **maximum limit** in size of **10,000**. That is 10,000 sub-jobs per array job.

The resources you request apply to each individual sub-job, not the entire array. For example, if you request 32 cores and 32GB of RAM in the jobscript, that will allocate 32 cores and 32GB of RAM to each sub-job. See more in the [strategies for writing array jobs section](#strategies-for-writing-array-jobs).


The system will run individual sub-jobs as soon as resources become available. Occasionally, the system may re-queue running sub-jobs to free resources for larger jobs. Always write your jobscripts with this in mind. For example, consider what would happen if the re-run subjob detects partial output from a previous run. 

### Stepping factors in array jobs

You can make use of a stepping factor when submitting array jobs. This means that only a given interval of the entire array job will be run. For example:

```bash
#PBS -J 1-8:2
```

This will produce sub-job indices of 1, 3, 5 and 7.

### Limiting array jobs

You can also limit array jobs down so that only a given number can run at any given time.

```bash
#PBS -J 1-20%2
```

This would allow only 2 array jobs to run at any given time. 


## Monitoring array jobs

When execution of the sub-jobs of an array job has begun, the `qstat` listing will show the state of the array job as "B" rather than "R". To see the progress through the array, use `qstat -t [jobid]` or look at the comment field in the output of `qstat -f [jobid]`.

## Strategies for writing array jobs

**Please note that when you write a job script for an array job, you should think of resource request for each individual sub-job and not the full array job**

For example, let us consider that you want to run an array job with 5 sub-jobs. Assume that each sub-job requires 8 cpus and 96 GB of RAM, then your job script should look like below.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=8:mem=96gb
#PBS -l walltime=01:00:00
#PBS -J 1-5
 
cd $HOME/project_1
 
my_program my_input_file
```

With this script, you are running an array job with 5 sub-jobs. However, as it currently is, each sub-job will run the exact same program with the exact same input file. The script needs to be modified so that each sub-job will use a different input file (this could also be any parameter needed by my_program).

There are several ways to achieve this making use of variable `$PBS_ARRAY_INDEX`:

### Use A Convenient Naming Convention For Input Files

The simplest method to use is to create an individual input file for each sub-job and use a naming convention that is easy to include in the job script. For instance, if you were to create 5 different input files with the names:

```
my_input_file_1
my_input_file_2
my_input_file_3
my_input_file_4
my_input_file_5
```

You could then modify the job script to contain:

```
my_program my_input_file_${PBS_ARRAY_INDEX}
```

### Use PBS_ARRAY_INDEX to Select Input from an Arbitrary List

Sometimes it would be inconvenient to rename existing input files to follow a naming convention as in the previous example. Let's consider the case where you have input files with the following names:

```
my_first_input_file
my_second_input_file
my_third_input_file
my_fourth_input_file
my_fifth_input_file
```

The names follow a pattern but not one that can easily be used with the numeric characters stored by `PBS_ARRAY_INDEX`. Instead you can use `PBS_ARRAY_INDEX` to specify an arbitrary index into a list of file names:

```
my_program $(ls my_*_input_file | sed -n "${PBS_ARRAY_INDEX}p")
```

The expression inside of `$()` will return a single file name depending on the value of `PBS_ARRAY_INDEX`. Note that, because `ls` lists files alphabetically, in this case the first sub-job (when `PBS_ARRAY_INDEX=1`) will use my_fifth_input_file and so on.

This approach can similarly be adopted if you have a file containing the list of input file names or parameters expected by your program:

```
my_program $(sed -n "${PBS_ARRAY_INDEX}p" input_file_list)
```

### Pass PBS_ARRAY_INDEX as a Command Line Argument

In the case that you are able to modify `my_program`, it is possible to make `PBS_ARRAY_INDEX` available via a command line argument. How this works depends on the programming or scripting language used and basic examples for a few are shown below. Once you have `PBS_ARRAY_INDEX` it is then up to you to use it within your script/program to choose the correct input file or parameter value. If you modify your job script to:

```
my_program $PBS_ARRAY_INDEX
```

The value of `PBS_ARRAY_INDEX` can then be accessed.

#### In Python

```python
import sys
index = int(sys.argv[1])
```

#### In R

```R
args <- commandArgs(trailingOnly=TRUE)
index=as.numeric(args[1])
```

#### In C

```C
int main(int argc, char **argv)

{

	const index=atoi(argv[1]);
}
```

#### In Matlab

You can pass `PBS_ARRAY_INDEX` to your matlab script with:

```console
matlab -r "index=$PBS_ARRAY_INDEX;myscript"
```
