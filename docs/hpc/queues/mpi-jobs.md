# MPI Jobs

## What is MPI
MPI or Message Passing Interface is a standardised message passing protocol for parallel computing. It enables multiple processes or tasks to communicate between each other, often between multiple compute nodes. Most of the applications we (Imperial RCS) see running on more than one compute node, utilise MPI to do so.

MPI is often combined with other parallel technologies such as OpenMP (or other threading technologies) to create so called Hybrid Programs.

## MPI Jobs with PBSPro
An MPI job can be configured with the standard #PBS resource specification:

```bash
#PBS -lselect=N:ncpus=Y:mem=Z
```

and then run with:

```bash
module load mpi
mpiexec a.out
```

This will start NxY MPI ranks, with Y ranks on each of N distinct compute nodes.

### The eternal dilemma, "mpirun" or "mpiexec" ?

The recommendation to use "mpiexec" wrapper or "mpirun" wrapper will depend on what application you are using and what MPI version it was compiled against. 

* If you are using any version prior to "mpi/intel-2019.8.254" then the recommendation would be to use "mpiexec". i.e. mpiexec myprogram

* If you are compiling with a newer MPI version you would need to use "mpirun". See section below for details.  i.e. mpirun myprogram


If you have any issues running your program or are not sure what to use, please get in touch with our support team following these guidelines. 

### Running a program with mpiexec 

mpiexec does not require any arguments other than the name of the program to run. For example:

```bash
mpiexec a.out [program arguments]
```

That being said, you can use the "-n" flag to specify the number of processes. This is useful if you would like to decrease the number of mpi ranks, whilst still selecting the same number of "ncpus" in the jobscript.

```bash
mpiexec -n 8 a.out [program arguments]
```

### Note in particular:

It's not necessary to add "-n" or any other flag to specify the number of ranks. If a specific rank count is required and cannot be exactly specified with N x P, use the optional flag “-n <num>”. The number of ranks requested this way must be no more than N x P
it isn't necessary to specify the full path to the program: mpiexec will search PATH for the program

## Compiling Your Own MPI Code

### New MPI Versions and Compilation of Custom Code

If you have custom MPI code, we recommend compiling using the following MPI versions:

* oneapi/mpi/2021.4.0

The following will require pre-loading module "tools/dev":

* iimpi/2020a
* iimpi/2020b
* iimpi/2021a
* iimpi/2021b

If you are compiling with any of the above, please use the wrapper `mpirun` instead of `mpiexec`.

Below is an example of running a job you've compiled yourself on CX3.

```bash
#PBS -l walltime=08:00:00
#PBS -l select=1:ncpus=128:mem=250gb
 
module load tools/prod
 
module load iimpi/2021b
 
export FI_MLX_IFACE=eth0
 
 
cd $PBS_O_WORKDIR
 
mpirun IMB-MPI1
```

## Hybrid Programs

Occasionally you may wish to run a hybrid program: one which combines MPI and OpenMP or other threading scheme. In this case, you may control the number of MPI ranks and  threads placed on each node with the additional directives `mpiprocs` and `ompthreads`. For example:

```bash
#PBS -lselect=N:ncpus=Y:mem=Z:mpiprocs=P:ompthreads=W
```

This will cause N nodes with Y ncpus to be allocated to the job. On each node there will be P MPI ranks and each will be configured to run W threads. You should ensure that PxW <= Y or the job may be aborted by PBS for exceeding its stated requirements.

When omitted these parameters assume the default values of `mpiprocs==ncpus` and `ompthreads==1`

## Capability Jobs

Capability jobs are those that utilise two or more compute nodes, typically utilising a high performance/low latency interconnect such as Infiniband to communicate between the compute nodes.

### CX3 Capability System

The CX3 capability queue provides users a way of testing multi-node jobs and workflows prior to apply for access to [HX1](../pilot/hx1.md). The following details are for the CX3 Legacy system, for the capability queue on CX3 Phase 2, please consult the [CX3 Phase 2 guide](../pilot/cx3-phase2.md).

If you are regularly running multi-node jobs and have tested the workflow, we strongly recommend you apply for access to [HX1](../pilot/hx1.md).

#### Node Specification

The following table lists the specification of each node.

* **CPU**: AMD 7742
* **Cores per node**: 128
* **Available RAM per node**: 920GB
* **Interconnect**: 100GbE (ethernet)

#### Queue Specification

* **Nodes in Queue**: 28
* **Maximum nodes per job**: 8
* **Maximum cores per job**: 1024

We've set these limits because our own testing has shown that above 8 nodes, jobs can struggle with the internode communication due to the use of ethernet rather than InfiniBand. If your workflow is working but you need to use more cores per job, please consider applying for access to [HX1](../pilot/hx1.md).

#### Template Job Scripts
Please make sure you read the [CX3 Best Practice section](#best-practice-with-the-cx3-capability-queue) before submitting any jobs.

Below are some job templates that you can use for submitting jobs in this CX3 capability queue.

##### CX3 Capability 2 nodes 128 tasks per node

```bash
#PBS -l walltime=08:00:00
#PBS -l select=2:ncpus=128:mem=250gb
 
module load tools/prod
 
module load iimpi/2021b
 
export FI_MLX_IFACE=eth0
 
cd $PBS_O_WORKDIR
 
mpirun IMB-MPI1
```

##### CX3 Capability 2 node 64 tasks per node 2 OpenMP threads per task (hybrid)

```bash
#PBS -l walltime=08:00:00
#PBS -l select=2:ncpus=128:mem=250gb:mpiprocs=64:ompthreads=2
 
module load tools/prod
 
module load iimpi/2021b
 
export FI_MLX_IFACE=eth0
 
cd $PBS_O_WORKDIR
 
mpirun IMB-MPI1
```

##### CX3 Capability 2 nodes 32 tasks per node 4 OpenMP threads per task (hybrid)

```bash
#PBS -l walltime=08:00:00
#PBS -l select=2:ncpus=128:mem=250gb:mpiprocs=32:ompthreads=4
 
module load tools/prod
 
module load iimpi/2021b
 
export FI_MLX_IFACE=eth0
 
cd $PBS_O_WORKDIR

mpirun IMB-MPI1
```

#### Best Practice with the CX3 Capability Queue

* Make sure that you pack your jobs in as few nodes as possible. For example if you previously requested 6 nodes with 56 tasks each (for a total of 336 tasks), consider either requesting 2 nodes with 128 tasks each (for a total of 256 MPI tasks or 3 nodes with 128 tasks each (for a total of 384 tasks).
* While these nodes will run software built using the Intel MPI and Compiler, we strongly recommend rebuilding your software using GCC and OpenMPI as we observe greater performance with these. Please raise a ticket with us if you think it may be possible to rebuild your software and need assistance.
* The large number of CPUs in each compute node means that there can be performance issues if your application is memory intensive. If your application supports running in hybrid MPI/OpenMP mode, we therefore recommend that you test your application to see if it performs better with 64 or 32 tasks per node, 2 or 4 OpenMP threads per task, accordingly.
* All MPI applications have a point whereby the job takes longer to run if you request more tasks. It is therefore important that you run job scaling tests to check the performance of your application on these compute nodes before running a programme of work.