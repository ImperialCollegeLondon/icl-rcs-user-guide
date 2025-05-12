# Best Practice

!!! info

    This page has been partially rewritten for CX3 Phase 2.

This page is intended as best practice guidelines for using the RCS. These recommendations are generally applicable for daily use (and may apply on HPC services offered elsewhere), but particularly important to keep in mind at times of reduced support, such as when the college closes during holiday periods.

## Running applications on the login nodes

The login nodes act as the gateways to our cluster and are where users connect to remotely. From these login nodes, users can manage their files, manage their environments, do some light compilation or submit their jobs. The most important thing one needs to remember is that the login nodes are a **shared resource** and some consideration needs to be exercised when using them.

Users must not use the login nodes to run their jobs/workflows. Computational intensive commands (multi-threaded, memory-intensive, filesystem intensive or long running commands) that run on login nodes (including workflows run with Visual Studio) that affect performance may be **de-prioritized** or **terminated**. These commands should be run either within a batch job, or by using interactive services ([Jupyter](./applications/guides/jupyter.md) or [Open On Demand](./applications/guides/openondemand.md)). 

The resources available (memory and cores) to users on the login nodes are capped. Any users trying to run their workflows on these systems will get very low performance. Users will be penalized if they exceed these limits, and their applications may be terminated. As a general rule, if you are going to run something that uses multiple cpus, will use more than a few GBs of memory or run for more than a few minutes, then it should be run within a job.

If you need to test or debug your work, please consider submitting a short job or requesting an [interactive session](./queues/job-sizing-guidance.md#interactive).

## Heavy I/O operations on the login nodes

Running a filesystem (or I/O) intensive application or script on the login nodes will typically result in the login node appearing to slow down. For example, other users may find that running simple operations (such as `ls`) become very slow with long waits to see the results.

This is (often) caused by:

* File operations (`mv`, `rm`, `cp`, `ls`) involving many thousands of files or a large amount of data
* Data transfers (`scp`, `rsync`, `cp`)  involving many thousands of files or a large amount of data.

When we observe such a slowdown on the login nodes, we may suspend or terminate your processes to permit other users to continue with their work.

### Internal transfers:

When carrying out large transfers within the RDS (more than a few GB’s or expected to run for more than a few minutes), we recommend that you do so by submitting a job to the batch queue, ideally requesting a single node to carry out the task. You can then use your transfer program (`rsync`, `cp`, `mv`) within the batch script.

### External transfers:

When transferring from/to outside of the RDS, the recommendation is to mount the [RDS as a network drive](../rds/access/index.md) or to use [Globus](../rds/transferringdata/globus.md) to carry out these transfers.

## Array jobs

Certain workflows can cause high load on the filesystem and this can result in slowdown of access to all users. Because array jobs are intended to run many copies (potentially thousands) of the same workflow, if this is configured incorrectly or if each subjob is performing a lot of filesystem access, it can have a significant impact on the filesystem for all users. If you are planning to run array jobs, please consider the following:

### I/O operations

* Be careful if each subjob is going to be reading/writing to the same file. If many subjobs run in parallel, the system will struggle to keep track what processes are using the file. Consider copying the input file to `$TMPDIR` and reading from there (see section below "[Staging via $TMPDIR](#stage-via-tmpdir)").
* Not all applications have the same impact on the filesystem; certain applications that perform random reads can really stress the filesystem. Many bioinformatics applications, due to their nature, are notorious for this. The recommendation here would to stage your application data via `$TMPDIR`. See the "[Staging via $TMPDIR](#stage-via-tmpdir)" section below.

### Stage via $TMPDIR

* When your job starts, the current working directory will be a location on the local disk of the compute node. This is a temporary location created when the job starts and it is accessible via the environment variable `$TMPDIR`. 
* This location is not accessible from the login-nodes. Here `$TMPDIR` will just point to `$EPHEMERAL`.
* Many workflows, particularly those that do a lot of reading/writing will benefit from staging their workflow via `$TMPDIR`. This will bypass the RDS as your application will be reading/writing directly to local disk.
* The general approach in your jobscript is:
    * Copy the necessary input file(s) to `$TMPDIR`
    * Run your application reading the file(s) from `$TMPDIR` and writing out to `$TMPDIR`
    * At the end of the jobscript, copy the required output/results file(s) back to the location of your choosing.

Example:

```bash
#!/bin/bash
#PBS -l walltime=02:00:00
#PBS -l select=1:ncpus=1:mem=1gb
#PBS -N 1-10
 
module load anaconda3/personal
source activate env1
 
# Copy input file to $TMPDIR
cp $HOME/project1/inputs/sample_${PBS_ARRAY_INDEX}.sam $TMPDIR
 
# Run application. As we have not changed directory, currently location is $TMPDIR
samtools view -S -b sample_${PBS_ARRAY_INDEX}.sam > sample_${PBS_ARRAY_INDEX}.bam
 
# Copy required files back
cp $TMPDIR/sample_${PBS_ARRAY_INDEX}.bam $HOME/project1/outputs/
```

### Number of files

Be mindful of the amount of files you are working with (inputs, outputs, temporary files). Avoid having a large number of files ( > 10,000) in a single directory. See the section below "File Management".

You might need to break down your files into dedicated folders for inputs and outputs. 

### Array job log files

By default, PBS will create two log files per subjob (one for stdout and one for stderr). With large array jobs, this can cause the number of files in the submission directory to increase very quickly. If submitting large array jobs, consider redirecting the PBS log files to a different directory. You would specify the directory with the PBS parameters (`-e [dir]` and `-o [dir]`).

i.e.

```bash
#PBS -l walltime=02:00:00
#PBS -l select=1:ncpus=1:mem=1gb
#PBS -o /rds/general/user/slacalle/home/project1/logs_project1/
#PBS -e /rds/general/user/slacalle/home/project1/logs_project1/
```

Note that the directory needs to exist prior submitting the job.

Note that creating a "master" logs directory for all your submitted jobs' log files would defeat the purpose, as this would quickly populate with a large number of files. (see section "File Management" below). It is best to create a directory on a per-job basis.

### IMPORTANT

* If you are unsure how your workflow impact the filesystem, please follow the advice above to '[Stage via $TMPDIR](#stage-via-tmpdir)'
* If you are planning to start to run a new workflow as an array job, please do not run this during the holidays while the systems are unattended.

## File Management

We want to make users aware that certain types of workflows are known to currently cause issues for the stable operation of the entire service. Please be careful as your actions have an impact on all other users of our systems.

Particularly, we would like all users to be mindful of the following operations:

* Number of files/directories in single directory:
    * Avoid large number (>10,000) of files (or directories) in a single directory.  Any listing operations or wildcard operations (such as grep *) will be significantly slowed by many files in a directory. 
    * If needing to work with a large number of files, try and break the number down into smaller folders. 
* In programming:
    * Do not open a file then write a small amount of data then close many times a second. Always write to files in large chunks if possible.
* Using small files:
    * Any file < 8M will not leverage parallel access to disks and as such will only go at the speed of a single disk.

### Quotas

* Quotas are displayed when you login via ssh to the login-nodes. You can also use the quota command to display this information in the terminal at any time.
* Different storage spaces have different quotas. For more information please see [Data Management on HPC](getting-started/data-management-on-hpc.md)
* Apart from the storage quota, there is also a quota for maximum number of files. If you reach this quota for maximum number of files you will need to delete or archive off your data. A good approach to keep the number of files down is to package/archive (using tools such as gzip, tar, zip, 7za) data that is no longer needed or temporary files.
