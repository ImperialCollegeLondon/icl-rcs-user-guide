# CX3 Phase 2

!!! warning

    The content will continue to be updated inline with testing on the new platform.

CX3 Phase 2 is a project to migrate the CX3/CX1 HPC system to a new server management platform, enabling better management of the operating system (including more timely updates) and also enabling a significant upgrade job scheduler, ensuring better isolation of jobs when they are running. Currently the project is in testing and we will occasionally invite users to try the system and test their workflow. Eventually the entirety of the CX3 facility will move to this new platform.

The purpose of this document is to provide information on how to use CX Phase 2, particularly those differences as compared to the original CX3. Please remember that as this service is in a test phase, services including compute nodes are expected to fail from time to time.

## Access
Access to CX3 Phase 2 is controlled in the way that it is for the original CX3 - there are no additional access steps. However as the facility is in the test phase, you are using it at your own risk and we may not be able to immediately resolve any issue you may have.

## CX3 Phase 2 Specification
This information is correct as of Jan 2024.

* Compute Nodes
    * 24 * Intel compute nodes, each with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 512 GB RAM per node; 4 GB RAM per core
    * 18 * AMD Rome with 2x AMD EPYC 7742 (128 cores, 1TB RAM per node)
* GPU Nodes
    * 2 x Intel GPU nodes, each with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1 TB RAM, 2 x A100 40GB GPUs per node.
    * 1 x Intel GPU node, with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1 TB RAM, 4 x A40 48GB GPUs per node.

## Connecting to CX3 Phase 2
There are two login nodes dedicated to CX3 Phase 2. They can be accessed over ssh to either login-ai.cx3.hpc.ic.ac.uk or login-bi.cx3.hpc.ic.ac.uk. Once CX3 Phase 2 is brought into production, a single hostname will be provided to access the service.

Please be aware that these two login nodes are "Intel Icelake" and can only be accessed over ipv6. Please see our [Remote Access guide](../../remoteaccess.md) for advice if you have problems connecting the the service. 

## Storage
The RDS is directly connected to the CX3 Phase 2 in the same way as it is for the original CX3, therefore all file paths remain the same. Your home area on CX3 phase 2 is the same as the original CX3. Please see the following two links for further information:


* [Data Management on HPC](../getting-started/data-management-on-hpc.md)
* [Research Data Store](../../rds/index.md)

## Software

Software installed on the cluster is accessible on CX3 Phase 2 using the module system, as it is for the original CX3. However, the old modules are no longer accessible, only the modules built using EasyBuild are immediately available, along with some binary packages such as Matlab. If you require software from the old module system, please raise a ticket so we can check it's compatibility with the new platform.

## Job Submission

CX3 Phase 2 uses a new instance of PBS Pro for the management of jobs; this is the same instance as currently deployed on HX1 although be aware that you cannot submit jobs to HX1 from CX3 Phase 2 (the jobs will fail). There are some notable differences in job submission which are described below, please make sure you review this information before submitting jobs to the queue.

### Submitting Jobs

The qsub command is used to submit jobs. 

```console
qsub myjob.pbs
```

!!! warning

    Please do not try to submit an HX1 job from CX3 Phase 2 as the job will fail.

### Explanation of PBS Directives
The following table provides an explanation of what each directive means in the context of your resource request on CX3 Phase 2. 


| Directive | Description |
| --------- | ----------- |
| `select` | This is the number of compute nodes you wish to use in your job. Most users on CX3 phase 2 will probably just be wanting to use 1 compute node. select=4 means you are requesting 4 compute nodes |
| `ncpus` | This is the number of compute cores to allocate to your job, on each allocated compute node. ncpus=64 would allocate 64 cpus to your job. You should ALWAYS set this option irrespective of what type of job you want to run. |
| `mpiprocs` | This is the number of mpi processes you would like to run per allocated compute node. For example, if you want to run 64 mpi processors, you would set mpiprocs=64 |
| `ompthreads` | This is the number of openmp threads you want to set, or to be more specific, it sets the OMP_NUM_THREADS environment variable in your job. This is set to 1 by default.
| `mem` | This is the amount of memory to allocate to the job per compute node. |
| `cpu_type` | This is the type of cpu to allocate to your job. This is only required if you need your job to only run on the AMD Rome (cpu_type=rome) or the Intel Icelake (cpu_type=icelake) compute nodes. |
| `place`	| Sets how the processes for be places on the nodes. The default is "free" which puts them where-ever there is space |
| `ngpus`	| This is the number of GPU devices to allocate to your job. |
| `gpu_type` | This is the type of GPU to allocate to your job. For example gpu_type=a100 or gpu_type=a40. |

### Example Resource Requests for Jobs
#### Basic Jobs

##### 1 Core 1GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=1:mem=1gb
#PBS -l walltime=00:01:00
```

##### 4 Cores 16GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=16gb
#PBS -l walltime=00:01:00
```

##### 64 cores 100GB RAM
```bash
#!/bin/bash
#PBS -l select=1:ncpus=64:mem=100gb
#PBS -l walltime=00:01:00
```

This could be allocated to either an Intel Icelake or and AMD Rome compute node.

#### Whole Node Jobs
##### 64 cores 400GB RAM on Intel Icelake

This would allocate a whole node of an Intel Icelake compute node.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=64:mem=400gb:cpu_type=icelake
#PBS -l walltime=00:01:00
```

##### 128 cores 900GB RAM on AMD Rome

```bash
#!/bin/bash
#PBS -l select=1:ncpus=128:mem=900gb:cpu_type=rome
#PBS -l walltime=00:01:00
```

#### OpenMP Jobs
##### 4 Cores 2 OpenMP Threads 16GB RAM

This will set `OMP_NUM_THREADS=2` in your job.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:ompthreads=2:mem=16gb
#PBS -l walltime=00:01:00
```

#### MPI Jobs
##### 1 compute node 64 mpi tasks per node 200GB RAM per node

```bash
#!/bin/bash
#PBS -l select=1:ncpus=64:mpiprocs=64:mem=200gb
#PBS -l walltime=00:01:00
```

##### 2 compute nodes 128 mpi tasks per node 200GB RAM per node

```bash
#!/bin/bash
#PBS -l select=2:ncpus=128:mpiprocs=128:mem=200gb
#PBS -l walltime=00:01:00
```

#### Hybrid OpenMP/MPI Jobs

##### 2 compute nodes 128 cores allocated 64 mpi tasks per node 2 OpenMP threads per task 200GB RAM per node

```bash
#!/bin/bash
#PBS -l select=2:ncpus=128:mpiprocs=64:ompthreads=2:mem=200gb
#PBS -l walltime=00:01:00
```

#### GPU Jobs
##### GPU Specification
| GPU Type | Single Precision<br>TFLOPS | Double Precision<br>TFLOPS | Memory<br>GB | Memory Bandwidth<br>GB/s | CUDA Compute Capability | GPU Architecture |
| -------- | -------------------------- | -------------------------- | ------------ | ------------------------ | ----------------------- | ---------------- |
| A100 PCIe 40GB | 19.5 | 9.7 | 40 |1,555 | 8.0 | Ampere |
| A40 | 19.2 | 0.5 |40 | 696 | 8.6 | Ampere | 

##### 4 cores 1 GPU 64GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=64gb:ngpus=1
#PBS -l walltime=00:01:00
```

##### 4 cores 1 A40 GPU 64GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=64gb:ngpus=1:gpu_type=A40
#PBS -l walltime=00:01:00
```