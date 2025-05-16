# Running your first job

The Imperial HPC facility is a batch processing system. Rather than carrying out work directly on the command line, jobs are submitted to the queue of a work manager (or resource scheduler) where they are held until compute resource become free. A job is defined by a shell script that contains all of the commands to be run.

The Imperial HPC facility uses the [PBSPro workload manager](https://altair.com/pbs-professional/), which some existing HPC users may be familiar with using on other HPC facilities.


## Anatomy of a job script

The very first line of a PBSPro job submission script specifies the interpreter or shell that should be used to execute the script. This line is sometimes called a "shebang" line or "hashbang" line. At Imperial we only support using the default Bash interpreter so this line should also be,

```bash
#!/bin/bash
```

Every job script must include two lines that describe the resources required by the work. The first specifies the maximum amount of time the job will be allowed to run for in `hours:minutes:seconds`:

```bash
#PBS -lwalltime=HH:MM:00
```

The second line gives the the number of cores `N` and memory `M` that the job needs. These values are per-node and in this example, only a single node is requested:

```bash
#PBS -lselect=1:ncpus=N:mem=Mgb
```

You should only request more than one node if your application is able to run across multiple nodes.

See [job sizing guidance](../queues/job-sizing-guidance.md) for more advice on resource requests.

Next comes the `module load` command, which loads the software you need for your job into the environment; by default, your job should have access to our "production" module environment. The following example then uses the `module load` command to load a version of Python.

```bash
module load Python/3.12.3-GCCcore-13.3.0
```

For more information on loading modules, please see the [Loading Applications](../applications/index.md) page.

The initial working directory when your job starts will be your `$HOME` directory; it is important to be aware of this when you are writing submission scripts as you may need to change the working directory while running commands. Within your job environment, you will also have access to two environment variables which you may find useful:

* `$PBS_O_WORKDIR` - this is the directory from which your job was submitted.
* `$TMPDIR` - this is a temporary directory created on the local disk of the compute node your job is running on. This directory is only accessible from the compute node itself and is deleted when your job ends.

For this simple example we will stick to using `$PBS_O_WORKDIR` but if the application you are running in your job is accessing the file system a lot, it would be best to copy data to `$TMPDIR` and work on it locally.

```bash
cd ${PBS_O_WORKDIR}
```

Next come the commands for the program you actually want to run, for example:

```bash
python myprog.py path/to/input.txt
```

So putting it all together we might get something like the following, where we are asking for 2 cpus and 4 gb of ram on a single node and then running a Python scirpt.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=2:mem=4gb
#PBS -l walltime=00:01:00
#PBS -N hello_world

module load Python/3.12.3-GCCcore-13.3.0

cd $PBS_O_WORKDIR
 
python myprog.py path/to/input.txt
```

Open a file with a text editor (nano, vim, emacs) and save it as job.sh. You can then submit this with 

```console
[user@login ~]$ qsub job.sh
```

## Copying to local storage

As mentioned in the previous section, your job will start in your `$HOME` directory. If your workflow has a lot of file system access (reading and/or writing - IO) it is better to copy data to the `$TMPDIR`, process it and then finally, copy any output files back from `$TMPDIR` to permanent storage in your `$HOME` directory (or RDS project directory if you have access to one). Have a look at the following example that stages a [BAM](https://en.wikipedia.org/wiki/Binary_Alignment_Map) file to `$TMPDIR` and then copy backs the result. 

```bash
#!/bin/bash
#PBS -l walltime=02:00:00
#PBS -l select=1:ncpus=2:mem=4gb
  
module load SAMtools/1.19.2-GCC-13.2.0
  
# Copy input file to $TMPDIR
cp $HOME/project1/inputs/some_sample.bam $TMPDIR
  
# Change to the $TMPDIR
cd $TMPDIR

# Run application
samtools view -S -b some_sample.bam > sample_output.bam
  
# Copy required files back
cp $TMPDIR/sample_output.bam $HOME/project1/outputs/
```

Please be aware that `$TMPDIR` is on the local disk of the compute nodes and is shared between users whose jobs are running on the compute node. If you are likely to need more than 100 GB of temporary space per job, please consider discussing with us first so we can advise you on the best way to run your jobs.

## Choosing job resources

It's very important that you accurately specify the jobs' resource requirements in the `#PBS` directives.

Advice on choosing the right resource requirements for your work is in our [job sizing guidance](../queues/job-sizing-guidance.md). As a general rule, the smaller the resource request, the less time the job will spend queuing.

## Submitting and monitoring a job

You can submit a job with the `qsub` command. For example:

```console
[user@login ~]$ qsub your_job_script
403036.pbs-7
```

If the submission is successful, the unique ID for the job will be returned; in the above example the job ID is `403036.pbs-7`.

You can monitor your job submissions using the `qstat` command. Without any options, qstat will list all jobs from all users currently running and queued on the system. You can narrow this list down to your own user by providing the `-u user` option to qstat. For example:

```console
[user@login ~]$ qstat -u user

pbs-7:
                                                            Req'd  Req'd   Elap
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
403063.pbs-7    user v1_smal* hello_wor*    --    1   1    1gb 00:01     R   --
403064.pbs-7    user v1_smal* hello_wor*    --    1   1    1gb 00:01     R   --
403065.pbs-7    user v1_smal* hello_wor*    --    1   1    1gb 00:01     Q   --
```

The S column in the output tells you the status of your job. A status of "Q" means the job is currently queued whereas a status of "R" means the job is running. You may occassionally see other status values for your jobs such as "H" meaning your job has is held or "B" which means that it is an array job.

When a job finishes, it disappears from the queue. Any text output is captured by the system and returned to the submission directory, in two files named after the jobscript and with the job id as suffix e.g.:

```console
[user@login ~]$ ls
your_job_script         # The job script
your_job_script.o403036 # The output file
your_job_script.e403036 # The error file
```

If you need to delete a job, ether while it is still queuing, or running, use the `qdel` command such as

```console
[user@login ~]$ qdel 403120.pbs-7
```

## Where to go on from here

Now that you know how to write and submit a job script, there are a number of sections of in the documentation which you may find useful:

* [Queueing System](../queues/index.md) - from here you will find out more information on how the queuing system works and the different classifications of jobs. 
* [Job sizing guidance](../queues/job-sizing-guidance.md) - this page will provide you advice on the resource requests you make to the job scheduler.
* [Best practice](../best-practice.md) - this page will provide you general best practice for running jobs on the HPC service
* [Applications](../applications/index.md) - from this section you will find links to advice on how to run certain applications on the HPC facility.
* [Research Data Store](../../rds/index.md) - this section gives you details on using the Research Data Store.
