## Job Submission

Job submission on HX2 differs slightly, as it uses the [SLURM Workload Manager](https://slurm.schedmd.com/) as its batch scheduler instead of PBS Pro. Slurm is already widely used at many central HPC facilities across UK universities.

### Key commands

| Slurm Command | PBS Pro equivalent | Description |
| ------------- | ------------------ | ----------- |
| `sbatch` | `qsub` | Submit a job script to the queue |
| `squeue` | `qstat` | Report the state of jobs in the queue |
| `scancel` | `qdel` | Cancel a job in the queue |

### Basic job script

The following shows an example of a basic job script for slurm. It will ask for 1 cpu (based on 1 task x 1 cpus-per-task) for 5 minutes, with a total of 1 GB of RAM. When the job starts it will run `./my_program`.

```bash
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH --time=00:05:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=1G

./my_program
```

### Submitting Jobs

A submission script can be submitted to the queue using the `sbatch` command. As an example:

```bash
$ sbatch run_my_program.slurm
Submitted batch job 12345
```

When the submission script has been successfully accepted by the queue, the job number will be displayed as an output to sbatch.

### Job Sizing Guidance

The final partition layout is still being developed. Note that on standard compute nodes, jobs are limited to 142 out of the 144 available cores; 2 cores are set aside to support the Weka file system and operating system. This is increased to 4 cores on GPU nodes, so a maximum of 60 cores can be requested.

The following partitions are supported:

| Queue | Use Cases | Nodes per job | No. of cores per node<br>(ncpus) | Mem per node<br>(GB) | Walltime<br>(hrs) |
| ----- | --------- | :-------------: | ---------------------------- | -------- | -------------- |
| [small](#small) | Low core jobs | 1 | 1 - 16 | 1 - 128 | 0 - 72 |
| [medium](#medium) | Single-node jobs | 1 | 17 - 72 | 1 - 1000 | 0 - 72 |
| [large](#large) | Whole node jobs | 1 | 73 - 142 | 1 - 2000 | 0 - 72 |
| [capability](#capability) | Multi-node jobs | 2 - 8 | 1 - 142 | 1 - 2200 | 0 - 72 |
| [interactive](#interactive) | Interactive CPU jobs | 1 | 1 - 16 | 1 - 64 | 0 - 8 |
| [interactive-gpu](#interactive-gpu) | Interactive GPU jobs | 1 | 1 - 16 | 1 - 64 | 0 - 8 |
| [gpu](#gpu) | Main queue for gpu jobs* | 1 | 1 - 60 | 1 - 1400 | 0 - 72 |

\* Please see details for specific queues below as there may be additional restrictions or limitations.

#### small
This queue is for low-core jobs, up to 16 cores and 128gb of RAM. Jobs in this queue typically make use of OpenMP or multi-threading to parallelise workflows.

#### medium
This queue is jobs using up-to 72 cores and/or 1TB of RAM. This is half the CPU's available on one of the nodes.

#### large
This queue is for whole node jobs, using an entire node - 142 cores and 2TB of RAM.

#### capability
This queue is for larger multi-node jobs, using up to 8 entire nodes at once. Workflows in this queue will normally be using technologies such as MPI to communicate between all nodes.



#### interactive
An example of how to run an interactive job can be found below.

```console
[user@login ~]$ srun --nodes=1 --ntasks-per-node=1 --cpus-per-task=16 --mem=32GB --time=08:00:00 --partition=interactive --pty bash
```

#### interactive-gpu
An example of how to run an interactive GPU job can be found below.

```console
[user@login ~]$ srun --nodes=1  --ntasks-per-node=1 --cpus-per-task=16 --mem=32GB --time=08:00:00 --gpus=1 --partition=interactive-gpu --pty bash
```

!!! warning 
	You can only run one interactive job at a time. This means you can either run an interactive CPU job, or an interactive GPU job at once, not at the same time.

#### gpu
There is an additional limit of 12 GPU's total per user on the gpu queue to allow for fair usage of the GPUs.



## CX3 cluster Limits

To ensure fair access to resources for all researchers there are a couple of limits that apply across all queues:

* 1 Jupyterhub or Open Ondemand job
* Maximum of 500 CPUs running across all jobs, including interactive jobs
* Maximum of 12 GPUs running across all jobs, including interactive jobs
* Maximum array size in any queue is 10000.

## Explanation of PBS Directives
The following table provides an explanation of what each directive means in the context of your resource request on CX3 Phase 2.

| Directive | Description |Default|
| --------- | ----------- |---------|
| `select` | This is the number of compute nodes you wish to use in your job. Most users on CX3 phase 2 will probably just be wanting to use 1 compute node.| 1|
| `ncpus` | This is the number of compute cores to allocate to your job, on each allocated compute node. ncpus=64 would allocate 64 cpus to your job. | 1|
| `mpiprocs` | This is the number of mpi processes you would like to run per allocated compute node. For example, if you want to run 64 mpi processors, you would set mpiprocs=64 |1|
| `ompthreads` | This is the number of openmp threads you want to set, or to be more specific, it sets the OMP_NUM_THREADS environment variable in your job. |1|
| `mem` | This is the amount of memory to allocate to the job per compute node. |1GB|
| `cpu_type` | This is the type of cpu to allocate to your job. This is only required if you need your job to only run on the AMD Rome (cpu_type=rome) or the Intel Icelake (cpu_type=icelake) compute nodes. | Not set|
| `place`       | Sets how the processes for be places on the nodes. The default is "free" which puts them where-ever there is space |free|
| `ngpus`       | This is the number of GPU devices to allocate to your job. | 0|
| `gpu_type` | This is the type of GPU to allocate to your job. For example gpu_type=A100 or gpu_type=L40S. | Not set|

The general advice it to set the resources to what is needed for a job but to not over specify. For example, don't set CPU type if your job can run fine or Rome or Icelake. This ensures your job starts as early as possible.

## Example Resource Requests for Jobs
### Basic Jobs

#### 1 Core 1GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=1:mem=1gb
#PBS -l walltime=00:01:00
```

#### 4 Cores 16GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=16gb
#PBS -l walltime=00:01:00
```

#### 64 cores 100GB RAM
```bash
#!/bin/bash
#PBS -l select=1:ncpus=64:mem=100gb
#PBS -l walltime=00:01:00
```

This could be allocated to either an Intel Icelake or and AMD Rome compute node.

### Whole Node Jobs

#### 128 cores 900GB RAM on AMD Rome

This would allocate a whole node of an AMD Rome compute node.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=128:mem=900gb:cpu_type=rome
#PBS -l walltime=00:01:00
```

#### 64 cores 400GB RAM on Intel Icelake

This would allocate a whole node of an Intel Icelake compute node.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=64:mem=400gb:cpu_type=icelake
#PBS -l walltime=00:01:00
```

### OpenMP Jobs
#### 4 Cores 2 OpenMP Threads 16GB RAM

This will set `OMP_NUM_THREADS=2` in your job.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:ompthreads=2:mem=16gb
#PBS -l walltime=00:01:00
```

### MPI Jobs
Multi-node MPI jobs running that use our `intel` modules follow a similar behaviour to that on HX1.
You will need to specify the `mpirun -v6` flag in order to avoid bootstrap errors.
This is **not** necessary for single-node MPI jobs.
#### 1 compute node 64 mpi tasks per node 200GB RAM per node

Note: You must specify both ncpus and mpiprocs.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=64:mpiprocs=64:mem=200gb
#PBS -l walltime=00:01:00
```

#### 2 compute nodes with 128 mpi tasks total and 200GB RAM per node

The number in ncpus and mpiprocs is per-node so here we have a total of 128 mpi processes.

```bash
#!/bin/bash
#PBS -l select=2:ncpus=64:mpiprocs=64:mem=200gb
#PBS -l walltime=00:01:00
```

### Hybrid OpenMP/MPI Jobs

#### 2 compute nodes 128 cores allocated 32 mpi tasks per node 2 OpenMP threads per task 200GB RAM per node

```bash
#!/bin/bash
#PBS -l select=2:ncpus=64:mpiprocs=32:ompthreads=2:mem=200gb
#PBS -l walltime=00:01:00
```

### GPU Jobs

Note that the GPU nodes have less CPUs than the standard compute nodes. Also while all GPU types are given here, the A40 cards can only be used on [Jupyterhub](https://jupyterhub-11.rcs.ic.ac.uk/).

#### GPU Specification

|  | [L40S PCIe 48 GB](https://resources.nvidia.com/en-us-l40s/l40s-datasheet-28413) | [A100 PCIe 40GB](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/a100/pdf/nvidia-a100-datasheet.pdf) | [A40](https://images.nvidia.com/content/Solutions/data-center/a40/nvidia-a40-datasheet.pdf) |
| -------- | -------- | -------- | -------- |
| FP64 Double Precision (Tensor) TFLOPS | N/A | 19.5  | N/A |
| TF32 Single Precision (Tensor) TFLOPS | 183  | 156  | 74.8 |
| FP64 Double Precision TFLOPS | N/A | 9.7  | N/A |
| FP32 Single Precision TFLOPS | 91.6 | 19.5 | 37.4 |
| Memory | 48 GB GDDR6 | 40 GB GDDR6 | 48 GB GDDR6 |
| Memory Bandwidth GB/s | 864 | 1,555 | 696 |
| CUDA Compute Capability | 8.9 | 8.0 | 8.6 |
| GPU Architecture | Ada Lovelace | Ampere | Ampere |


#### 4 cores 1 GPU 64GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=64gb:ngpus=1
#PBS -l walltime=00:01:00
```

#### 4 cores 1 L40S GPU 64GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=64gb:ngpus=1:gpu_type=L40S
#PBS -l walltime=00:01:00
```

#### single core jobs

 A single core submission script would look like the following:

```bash
#!/bin/bash
#SBATCH --job-name=test_job
#SBATCH --time=00:05:00
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=1

./my_program
```

#### Multi-threaded jobs (OpenMP/shared memory)

If the program uses threads on a single node:

```bash
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=8
```

What this means:

* `--nodes=1` -> one node
* `--ntasks-per-node=1` -> one process per node
* `--cpus-per-task=8` -> that process can use 8 CPU cores (threads)

You may also need to set:

```
export OMP_NUM_THREADS=8
```

or

```
export OMP_NUM_THREADS=${SLURM_CPUS_PER_TASK}
```

To control threading within the OpenMP program. 

#### MPI or distributed tasks

If your job supports multiple independent process (such as MPI):

```bash
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --cpus-per-task=1
```

What this means:

* Slurm launches 8 processes
* Each process gets 1 CPU core

You would launch your processes with:

```
srun ./my_mpi_program
```

or 

```
mpirun ./my_mpi_program
```

In both cases, the mpi distribution must be suitably configured to work with Slurm (the centrally provided MPI distributions are already configured with this integration).

#### Hybrid MPI and threading

If each MPI rank uses multiple threads:

```
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --cpus-per-task=4
```

This means:

* 4 MPI processes are started
* Each MPI process gets 4 CPU cores (threads)
* a total of 16 CPU cores are allocated to the job

As with the [multi-threaded job](#multi-threaded-jobs-openmpshared-memory), you may need to set:

```
export OMP_NUM_THREADS=4
```

or

```
export OMP_NUM_THREADS=${SLURM_CPUS_PER_TASK}
```

### GPU Jobs

**GPU Node Example:**

```
#!/bin/bash
#SBATCH --job-name=example-job
#SBATCH --time=03:00:00
#SBATCH --ntasks-per-node=4
#SBATCH --gres=gpu:1
#SBATCH --mem=32GB
#SBATCH --partition=gpu
```

There is an additional limit of 12 GPU's total per user on the H200 queue to allow for fair usage of the GPUs.

#### Requesting Local Storage

Each node has some amount of local storage. This can be used as a scratch space for staging which is used only within a given job. By default this can be accessed with $TMPDIR and is limited to 1GB. This space is deleted when the job finishes and can not be recovered so make sure to copy any important files back to home when the job finishes. 

To request more scratch space, set the following flag:
`--gres=scratch:100g`

**CPU Node Example:**
```
#!/bin/bash
#SBATCH --job-name=example-job
#SBATCH --time=03:00:00
#SBATCH --cpus-per-task=4
#SBATCH --ntasks-per-node=1
#SBATCH --gres=scratch:100g
#SBATCH --mem=32GB
#SBATCH --partition=small
```

The above is an example of how to request scratch space on a CPU node, requesting 4 cores and 100GB of scratch space.

**GPU Node Example:**
```
#!/bin/bash
#SBATCH --job-name=example-job
#SBATCH --time=03:00:00
#SBATCH --cpus-per-task=4
#SBATCH --ntasks-per-node=1
#SBATCH --gres=gpu:1,scratch:100g
#SBATCH --mem=32GB
#SBATCH --partition=gpu
```

The above is an example of how to request scratch space on a GPU node, requesting 1 GPU and 100GB of scratch space.


### Example Slurm Jobs
#### MPI Jobs

#### Hybrid OpenMP/MPI Jobs

### MPI Distribution Specific Information

#### Intel MPI

#### OpenMPI

