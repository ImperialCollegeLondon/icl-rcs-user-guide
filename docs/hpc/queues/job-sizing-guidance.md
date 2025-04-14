# Job Sizing Guidance

!!! info

    This page **has** been rewritten for CX3 Phase 2.

## Submitting a job
For information about submitting a job to our systems, please see our [how it works page.](./how-it-works.md)

There is no need to specify a queue, simply define the job in terms of the resources it requires and the system will run it as soon as suitable resources become available. When more than one queue matches a job specification, PBS will place the jobs in the first queue by alphabetical order.

The following queues of jobs are supported:

| Queue | Use Cases | Nodes per job | No. of cores per node<br>(ncpus) | Mem per node<br>(GB) | Walltime<br>(hrs) |
| ----- | --------- | :-------------: | ---------------------------- | -------- | -------------- |
| [small24](#small) | Low core jobs 24h | 1 | 1 - 16 | 1 - 128 | 0 - 24 |
| [small72](#small) | Low core jobs 72h | 1 | 1 - 16 | 1 - 128 | 24 - 72 |
| [jupyter](#jupyter) | Queue for JupyterHub jobs* | 1 | 1, 4, 8 | 8, 32, 64 | 2, 4, 8 |
| [jupytergpu](#jupytergpu) | Queue for JupyterHub GPU jobs* | 1 | 4 | 32 | 8 |
| [medium24](#medium) | Single-node jobs 24h | 1 | 1 - 64 | 1 - 450 | 0 - 24 |
| [medium72](#medium) | Single-node jobs 72h | 1 | 1 - 64 | 1 - 450 | 24 - 72 |
| [large24](#large) | Whole node jobs 24h | 1 | 1 - 128 | 1 - 920 | 0 - 24 |
| [large72](#large) | Whole node jobs 72h | 1 | 1 - 128 | 1 - 920 | 24 - 72 |
| <s>largemem72</s> | <s>Large memory jobs</s> | <s>1</s> | <s>1 - 128</s> | <s>921 - 4000</s> | <s>0 - 72</s> |
| [gpu72](#gpu72) | Main queue for gpu jobs* | 1 | 1 - 64 | 1 - 920 | 0 - 72 |
| [capability24](#capability) | Multi-node jobs 24h | 2 - 4 | 1 - 64 | 1 - 450 | 0 - 24 |
| [capability48](#capability) | Multi-node jobs 48h | 2 - 4 | 1 - 64 | 1 - 450 | 24 - 48 |

\* Please see details for specific queues below as there may be additional restrictions or limitations.

#### small
This queue is for low-core jobs, up to 16 cores and 128gb of RAM. Jobs in this queue typically make use of OpenMP or multi-threading to parallelise workflows. 

#### jupyter
This queue is where [JupyterHub](https://jupyterhub-11.rcs.ic.ac.uk) jobs are run. There is a limit of 1 concurrent job per user across both jupyter queues.

#### jupytergpu
This queue has NVIDIA A40 (48GB) and NVIDIA RTX6000 (24GB) GPU's. There is a limit of 1 concurrent job per user across both jupyter queues.

#### medium
This queue is for single node jobs, using up to an entire node - 64 cores and 450GB of RAM.*

#### large 
This queue is for whole node jobs, using an entire node - 128 cores and 920GB of RAM.* 

#### gpu72
There is an additional limit of 12 GPU's total per user on the gpu72 queue to allow for fair usage of the GPUs. See the section on [GPU Jobs](#gpu-jobs) for further information.

#### capability
This queue is for larger multi-node jobs, using up to 4 entire nodes at once. Workflows in this queue will normally be using technologies such as MPI to communicate between all nodes.

#### interactive
While not listed in the table above, you can run an interactive job with the "-I" qsub flag. You would use this flag directly on the command line specifying the resources you need. e.g.:
```console
qsub -I -l select=1:ncpus=2:mem=8gb -l walltime=02:00:00
```
You should not request an interactive job longer than 8 hours, and should make sure to end your session once you are done as to not leave it idle.

**Note that on CX3 Phase 2, some nodes have up to 128 cores and 920GB of RAM, and some have up to 64 cores and 450GB of RAM. This is why there are separate [medium](#medium) and [large](#large) queues for them although technically both are "whole node queues".* 

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
