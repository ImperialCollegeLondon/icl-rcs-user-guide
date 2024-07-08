# Not enough slots to run your MPI program

Let us suppose that you want to run a MPI program on the cluster and you have requested the following resources in your job script:-

```bash
#PBS -l select=1:ncpus=4:mem=16gb
```

Next suppose that you try to run your mpi job by the following command

```bash
mpirun -np 4 ./your_mpi_program
```

You would typically assume that this should run just fine as you have requested 4 cores and you are running your job on 4 cores as well.

However, this may lead to the following error message.

```bash
There are not enough slots available in the system to satisfy the 4
slots that were requested by the application:

  ./your_mpi_program

Either request fewer slots for your application, or make more slots
available for use.

A "slot" is the Open MPI term for an allocatable unit where we can
launch a process.  The number of slots available are defined by the
environment in which Open MPI processes are run:

  1. Hostfile, via "slots=N" clauses (N defaults to number of
     processor cores if not provided)
  2. The --host command line parameter, via a ":N" suffix on the
     hostname (N defaults to 1 if not provided)
  3. Resource manager (e.g., SLURM, PBS/Torque, LSF, etc.)
  4. If none of a hostfile, the --host command line parameter, or an
     RM is present, Open MPI defaults to the number of processor cores

In all the above cases, if you want Open MPI to default to the number
of hardware threads instead of the number of processor cores, use the
--use-hwthread-cpus option.

Alternatively, you can use the --oversubscribe option to ignore the
number of available slots when deciding the number of processes to
launch.
```

Please do not use the `--oversubscribe` option as it may lead to performance degradation. This option will cause more number of mpi processes on the same physical core which causes the performance to degrade significantly.

The solution to this is pretty simple and involves adding one argument to your resource request in the job script. You need to add `:mpiprocs=4` to your resource request as shown below.

```bash
#PBS -l select=1:ncpus=4:mpiprocs=4:mem=16gb
```

By default, the value of `mpiprocs` is set to 1. If you do not specify this value, the scheduler will assume that you want to run only 1 mpi process on the number of cores you have requested. This is why you get the error message that there are not enough slots available in the system to satisfy the 4 slots that were requested by the application.

By specifying `mpiprocs=4`, you are telling the scheduler that you want to run 4 mpi processes on the 4 cores you have requested. This will allow you to run your mpi program without any issues.
