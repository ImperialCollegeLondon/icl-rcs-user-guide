# CX3 Legacy

This page contains information regarding the origin CX3 "legacy" facility for those users who have not transferred their workflow to the refreshed system yet. CX3 Phase 2 is a refresh of the CX3 platform, with an upgraded operating system and job scheduler. 

## Using SSH
### Hostname for SSH
```bash
login.hpc.imperial.ac.uk
```
Do note that due to security concerns, key-based authentication is disabled for the login-nodes. Users will need to login using their college username and password.

### Linux and MacOS
Linux and macOS users have the OpenSSH SSH client automatically installed on their operating system and this can be accessed from a terminal session.

Open a terminal session on your computer and run:
```bash
ssh username@login.hpc.imperial.ac.uk
```
at the command prompt, substituting your own College username and entering your password when prompted.

### Putty

Putty is the SSH client traditionally used by many on Windows and is installed on managed College computers. If you are intending to use Putty on your own (personal or unmanaged) computer, please make sure you install the latest version and only download from [https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). If you already have Putty installed, please make sure you update your version until at least 0.72.

### Command-line SSH clients

On Linux and macOS you have to use a terminal to connect using a regular SSH client with the "Enable X11 forward" flag "-X".

Your connect command will look like:

```
ssh -X username@login.hpc.ic.ac.uk
```

On other clients you will have to look for the "Enable X11 forwarding" option.

For example on Putty, you will need to navigate to Connection -> SSH -> X11 and activate the "Enable X11 forwarding".

## Classes of jobs

With the latest [job sizing guidance](#job-sizing-guidance), it is no longer necessary to know which queue to send your job to as this will automatically be determined by the job scheduler. However, it can still be useful to understand the different classes of jobs that may typically be run on an HPC system such as that provided by Imperial.

### High-Throughput

This job class typically refers to a high number of relatively small jobs. Ideally these jobs should fit within a single compute node. Often these jobs will utilise a single CPU core or small number of CPUs.

### High-End

This job class refers to those jobs that are typically requesting whole compute nodes to multiple compute nodes. They may utilise a mixture of technologies such as OpenMP for "threading" inside compute nodes and MPI for communication between compute nodes.

### Capability

This job class typically refers to those jobs that are run across many compute nodes (often 10 or more), normally using a technology such as MPI to communicate between them. Jobs such as this will take advantage of any fast and low latency interconnect between compute nodes such as Infiniband.

### Large Memory

These are jobs that require large amounts of memory in order to run. For Imperial College, this currently refers to compute jobs requiring more than 920GB of RAM.

### GPU
The GPU class of job refers to those jobs/applications that are utilising GPU technologies such as NVidia's CUDA to run computational work on GPU accelerator cards. See the section on GPUs in the [Job sizing guidance](#job-sizing-guidance) for further information.

## MPI Jobs

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
it isn't necessary to specify the full path to the program: mpiexec will search `PATH` for the program

## Conda 
### Anaconda3/personal

Long term users will remember the anaconda3/personal module and the attached documentation. This method relied on packages provided by Anaconda Inc. but in 2020 they changed their terms of service requiring all companies with more than 200 employees to buy a commercial license. As most workflows on HPC use the conda-forge repo anyway we are moving towards using the open source product provided by the same team that manage those repos. Users can continue to use their original environments if they install miniforge but they will have to be activated using the full path. For example, if you installed [Tensorflow](./tensorflow.md) following the old documentation you can still activate that environment with the following:

```
eval "$(~/miniforge3/bin/conda shell.bash hook)"
conda activate /rds/general/user/username/home/anaconda3/envs/tf2_env
```

If you are still using the old Anaconda3/personal module can we please ask that you [Disable the "defaults" channel](../applications/guides/conda.md#how-to-disable-the-defaults-channel)  as this may require the University to purchase a license for every user. More information on Anaconda's terms of service can be found at: [https://www.anaconda.com/pricing/terms-of-service-faqs](https://www.anaconda.com/pricing/terms-of-service-faqs)

## Job Sizing Guidance
The following queues of jobs are supported:

| Queue | Use Cases | Nodes per job | No. of cores per job<br>(ncpus) | Mem<br>(GB) | Walltime<br>(hrs) |
| ----- | --------- | :-------------: | ---------------------------- | -------- | -------------- |
| short8 | Short running jobs | 1 | 1 - 256 | 1 - 920 | 0 - 8 |
| [pqjupyter](#pqjupyter) | Queue for JupyterHub jobs* | 1 |1, 2, 4, 8 | 8, 16, 32, 64 | 8 |
| [pqood](#pqood) | Queue for Open OnDemand (RStudio) jobs* |1 |1, 2, 4, 8 |8, 16, 32, 64 | 8 |
| throughput72 | Low core jobs | 1 | 1- 8 | 1 - 100 | 9 - 72 |
| [medium72](#medium72) | Single-node jobs | 1 | 9 - 127 | 1 - 920 | 9 - 72* |
| [large72](#large72) | Whole node jobs | 1 | 128 - 256 |1 - 920 | 9 - 72* |
| [largemem72](#largemem72) | Large memory jobs* | 1 | 1 - 256 | 921 - 4000 | 0 - 72 |
| [gpu72](#gpu72) | Main queue for gpu job* | 1 - 4 | 1 - 256 | 1 - 920 | 0 - 72 |
| capability24 | Multi-node jobs 24h |2 - 8 | 64 - 128  | 1 - 920 |     0 - 24 |
| capability48 | Multi-node jobs 48h | 2 - 8 | 64 - 128 | 1 - 920 | 24 - 48 |

\* Please see details for specific queues below as there may be additional restrictions or limitations.

### pqjupyter

This queue is where JupyterHub jobs are run. There is a limit of 1 concurrent job per user.

### pqood

This queue is used by Open OnDemand (RStudio) jobs. There is a limit of 1 concurrent job per user.

### interactive

Unfortunatly it isn't possible to run interactive jobs on CX3 Phase 1. If you need an interactive session for debugging please use Jupyterhub/Open OnDemand or you can check out [cx3-phase2](../pilot/cx3-phase2.md).

### medium72

Job extensions are allowed on the medium queue only for jobs requesting the maximum walltime (72h). This is done via the self-service page. You will only be able to extend jobs when they are within 18 hours of reaching the walltime. You can only extend for an additional 24 hours at a time, up to a maximum of 144 hours. The job extension feature exists to be used sparingly, and isn't something to rely on being available at any particular time.

### gpu72

There is an additional limit of 32 GPUs total per user on the gpu72 queue to allow for fair usage of the GPUs. Please see our [GPU Jobs guide](./gpu-jobs.md) for how to run GPU jobs.

### largemem72

There is an additional limit of 16384 GB total per user on the largemem72 queue to allow for fair usage.

### large72

There is an additional limit of 5 running jobs per users on this queue to allow for fair usage. In addition, all nodes requesting 128 CPUS or more will be allocated a whole node as the CPUS from 129-256 are from SMT and are not from physical cores.

Please note there is a default limit of 50 concurrent jobs (queued or running) per user. Some queues may impose additional limits to guarantee fair-share and prevent monopolization.

### Advanced PBS Flags

PBS supports flags (also referred to as directives) in submission scripts that enable advanced users to give more details about the node(s) your job needs to run on. With the decommission of CX1 and CX2, and with CX3 Phase 1 consisting of mostly the same specification of compute node (AMD Rome, GPFS file system and ethernet interconnect) they now have limited usefulness on the current legacy HPC platform; the table below and examples are therefore provided here for reference purposes.

The [cx3-phase2](../pilot/cx3-phase2.md) facility has an updated set of flags/directives which can be reviewed in the documentation.

!!! warning

    We generally advise PBS flags/directives not to be left in submission scripts unless they are actively being used as they can increase queue times.

| Flag | Value | Description |
| ---- | ----- | ----------- |
| writabletmp | True/False | Some applications need to write to /tmp, (cx3 only) |
| filesystem_type | gpfs/nfs | Target only nodes with given filesystem type |
| cpu_type | rome | Target only nodes with given CPU type |
| gpu_type | RTX6000 | Select a specific type of GPU |
| interconnect | infiniband/ethernet | cx1/cx3 uses Ethernet while cx2 uses IB |

#### Flag Examples

##### Example 1

```bash
#PBS -lselect=1:ncpus=12:mem=50gb
#PBS -lwalltime=20:0:0
```

A job with more than 8 CPUs, more than 8 hours walltime and less than 920 GB of memory would be run in the **medium72** queue.

```bash
#PBS -lselect=1:ncpus=12:mem=50gb:cpu_type=rome
#PBS -lwalltime=20:0:0
```

Setting filesystem_type to GPFS would have the same effect as that is a distinguishing feature between the two queues.

##### Example 2

```bash
#PBS -lselect=2:ncpus=64:mem=50gb
#PBS -lwalltime=20:0:0
```

There are 2 multi-node queues, **capability24** and **capability48**. Capability queues are currently running on AMD Rome with 100 GbE ethernet. To target this system, request either 64 or 128 CPUs and up to 8 nodes. If you are regularly submitting multi-node jobs, please consider applying for access to the [HX1 capability cluster](../hx1.md).

## Which queue will I end up in?

### Single node job example - CPU

```bash
#PBS -lselect=1:ncpus=x:mem=32gb
#PBS -lwalltime=2:0:0
```

* 8 CPUs or less and 8 hours walltime or less then the job will go to **short8**
* 8 CPUs or less but walltime is more than 8 hours then the job will go to **throughput72**
* more than 8 CPUs and more than 8 hours walltime then job will go to **medium72** (see [Advanced PBS Flags](#advanced-pbs-flags) section above)
* more than 8 CPUs but less than 128 and more than 8 hours walltime then job will go to **medium72**
* more than 127, more than 8 hours walltime then job will go to **large72**

### Single node job example - Mem

```bash
#PBS -lselect=1:ncpus=2:mem=xgb
#PBS -lwalltime=2:0:0
```

* 8 CPUs or less, 8 hours walltime or less then the job will go to **short8**
* 8 CPUs or less but walltime is more than 8 hours while mem is 100 GB or less then the job will go to **throughput72**
* more than 8 CPUs but less than 128, more than 8 hours walltime and more than 100 GB then the job will go to **medium72**
* more than 127, more than 8 hours walltime then job will go to **large72**
* more than 920 GB of mem and the job will go to **largemem72**

### Multinode example

```bash
#PBS -lselect=x:ncpus=2:mem=100gb
#PBS -lwalltime=2:0:0
```
Please request whole nodes when using the capablity queue. If you need to use more than 1 node your job is likely using all of at least 1 resource on the nodes and so you should be requesting the whole node. The simplest method is to request all the CPUs.

* 64 or less CPUs and 2 or more nodes while walltime is 72 hours or less is not an accepted job format.
* 64 or 128 CPUs, 8 or less nodes while walltime is 48 hours or less then the job will go to **capability48**
* 64 or 128 CPUs, 8 or less nodes while walltime is 24 hours or less then the job will go to **capability24**
                                                                                                             
