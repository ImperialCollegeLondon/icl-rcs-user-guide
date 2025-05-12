# How much resources shall I ask for my job?

This is also one of the most common questions that many new users asks. Unfortunately, it is difficult for anyone to tell how much resource you should ask for in your job script. It is usually only after some trial and error and based on your experience, that you'd be able to have a better estimate of how much resource your job requires.

As a first step (and only for the first few runs), it is a good idea to ask for more resources.  You can use the following as a guideline

* If you are unsure about the walltime, you can ask for the maximum walltime (72 hours) which is a hard limit set for all the users in the college.
* If you are unsure about the RAM, ask for the maximum amount of RAM available on one node.
* You should always start with small runs that finish quickly and scale up gradually to know the best combination of resources required for your job.
* Use the `time` command to measure how much time it takes for your job to finish. For example, suppose that you are running a simple mpi program on 2 cores. Your job script may look like

```bash
#!/bin/bash
#PBS -N your_job_name
#PBS -l select=1:ncpus=2:mem=10gb
#PBS -l walltime=00:01:00
#PBS -o output.txt
#PBS -e error.txt
 
# Load the MPI module (adjust based on your cluster)
module load tools/prod
module load OpenMPI/4.1.4-GCC-12.2.0
 
# Change to the directory where the MPI program is located
cd $PBS_O_WORKDIR
 
# Execute the MPI program
time mpiexec -np 2 ./your_executable
```

At the end of your job, you will see something like the following in your error file.

```bash
real 0m3.743s
user 0m0.303s
sys 0m1.144s
```

Thus, you can see your job requires only 3 seconds of real wall clock time to run. You can use this as a guidance for your future jobs.

To further figure out how much resource would be required for your job, you can profile your code. For more details, please see Profiling an Application and more specifically Intel Application Performance Snapshot (APS). Doing this will give you an idea on how much resources does your job require and you can use that in your future runs.

For larger applications which may spawn over hundreds of cores on multiple nodes, it is even more important to benchmark your code (See [https://hpc-wiki.info/hpc/Application_benchmarking](https://hpc-wiki.info/hpc/Application_benchmarking) and [https://epcced.github.io/2021-04-19_PackagePerformance_Online/03-benchmarking/index.html](https://epcced.github.io/2021-04-19_PackagePerformance_Online/03-benchmarking/index.html) for details) and find out the right combination of resources. Otherwise, you might be wasting a lot of resources without gaining any significant advantage in terms of execution time. In many cases, your job may be under performing if you request higher resources than what your job is capable of efficiently running on.

!!! warning

    Some users may be tempted to use maximum wall time and maximum resources for their job without making an effort to understand their application and benchmark the same. This will effect them negatively in two ways
    
    1. They will experience longer wait time as mentioned in [Long Queue Times](./long-queue-times.md).
    1. Their run time may be longer as the increase in communication time between the cores may be much higher than the decrease in the walltime by using more number of cores.

With this, we complete our introductory guidance on how to ask for the right amount of resources for your job and we hope that you will follow this advice in your job submission process.
