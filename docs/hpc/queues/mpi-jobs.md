# MPI Jobs

!!! info

    This page **has** been rewritten for CX3 Phase 2.

## What is MPI
MPI or Message Passing Interface is a standardised message passing protocol for parallel computing. It enables multiple processes or tasks to communicate between each other, often between multiple compute nodes. Most of the applications we (Imperial RCS) see running on more than one compute node, utilise MPI to do so.

MPI is often combined with other parallel technologies such as OpenMP (or other threading technologies) to create so called Hybrid Programs.

## MPI Jobs with PBSPro
An MPI job can be configured with the standard #PBS resource specification:

```bash
#PBS -lselect=N:ncpus=X:mpiprocs=Y:mem=Z
```

and then run with:

```bash
module load gompi/2023b 
mpirun a.out
```

This will start N x Y MPI ranks, with Y ranks on each of N distinct compute nodes. 

### Running MPI with Intel
If you want to run a multi-node MPI job using any of the `intel` modules, you will need to include the `mpirun -v6` flag.
```bash
module load intel/2024a
mpirun -v6 a.out
```

### The eternal dilemma, "mpirun" or "mpiexec" ?

Use should use `mpirun` over `mpiexec` where possible as `mpirun` handles setting up the environment better, particularly in HPC environments.

If you have any issues running your program or are not sure what to use, please get in touch with our support team following these guidelines. 

### Running a program with mpirun 

`mpirun` does not require any arguments other than the name of the program to run. For example:

```bash
mpirun a.out [program arguments]
```

That being said, you can use the "-n" flag to specify the number of processes. This is useful if you would like to decrease the number of mpi ranks, whilst still selecting the same number of "ncpus" in the jobscript. This is also useful for [hybrid jobs](#hybrid-programs), where you use both OMP threads as well as MPI ranks.

```bash
mpiexec -n 8 a.out [program arguments]
```

## Compiling Your Own MPI Code

### New MPI Versions and Compilation of Custom Code

If you have custom MPI code, you can compile it using any of MPI modules, such as `intel` if you wish to use Intel's toolchain, or `gompi` if you prefer to use OpenMPI.

You can search for these modules with:
```bash
module spider intel
module spider gompi
```

Below is an example of running a job you've compiled yourself.

```bash
#PBS -l walltime=08:00:00
#PBS -l select=2:ncpus=64:mpiprocs=64:mem=250gb
 
module load tools/prod
module load intel/2023b
 
cd $PBS_O_WORKDIR
 
mpirun -v6 hello_world.exe
```

## Hybrid Programs

Occasionally you may wish to run a hybrid program: one which combines MPI and OpenMP or other threading scheme. In this case, you may control the number of MPI ranks and  threads placed on each node with the additional directives `mpiprocs` and `ompthreads`. For example:

```bash
#PBS -lselect=N:ncpus=Y:mem=Z:mpiprocs=P:ompthreads=W
```

This will cause N nodes with Y ncpus to be allocated to the job. On each node there will be P MPI ranks and each will be configured to run W threads. You should ensure that PxW <= Y or the job may be aborted by PBS for exceeding its stated requirements.

When omitted, these parameters assume the default values of `mpiprocs=1` and `ompthreads=1`

## Capability Jobs

Capability jobs are those that utilise two or more compute nodes, typically utilising a high performance/low latency interconnect such as Infiniband to communicate between the compute nodes.

### Capability System

The capability queue provides users a way of testing multi-node jobs and workflows prior to apply for access to HX1. If you are regularly running multi-node jobs and have tested the workflow, we strongly recommend you apply for access to [HX1](../hx1.md). 


#### Template Job Scripts
Please make sure you read the [Best Practice section](#best-practice-with-the-capability-queue) before submitting any jobs.

Below are some job templates that you can use for submitting jobs in this CX3 capability queue.

##### Capability 2 nodes, 128 tasks per node

```bash
#PBS -l walltime=08:00:00
#PBS -l select=2:ncpus=128:mem=250gb:mpiprocs=128
 
module load tools/prod
module load intel/2023b
 
cd $PBS_O_WORKDIR
 
mpirun -v6 IMB-MPI1
```

##### Capability 2 nodes, 64 tasks per node, 2 OpenMP threads per task (hybrid)

```bash
#PBS -l walltime=08:00:00
#PBS -l select=2:ncpus=128:mem=250gb:mpiprocs=64:ompthreads=2
 
module load tools/prod
module load intel/2023b
 
cd $PBS_O_WORKDIR
 
mpirun -v6 IMB-MPI1
```

##### Capability 2 nodes, 32 tasks per node, 4 OpenMP threads per task (hybrid)

```bash
#PBS -l walltime=08:00:00
#PBS -l select=2:ncpus=128:mem=250gb:mpiprocs=32:ompthreads=4
 
module load tools/prod
module load intel/2023b
 
cd $PBS_O_WORKDIR

mpirun -v6 IMB-MPI1
```

#### Best Practice with the Capability Queue

* Make sure that you pack your jobs in as few nodes as possible. For example if you previously requested 6 nodes with 56 tasks each (for a total of 336 tasks), consider either requesting 2 nodes with 128 tasks each (for a total of 256 MPI tasks or 3 nodes with 128 tasks each (for a total of 384 tasks).
* If your application supports running in hybrid MPI/OpenMP mode, we recommend that you test your application to see if it performs better with 64 or 32 tasks per node, 2 or 4 OpenMP threads per task, accordingly.
* All MPI applications have a point whereby the job takes longer to run if you request more tasks. It is therefore important that you run job scaling tests to check the performance of your application on these compute nodes before running a programme of work.
