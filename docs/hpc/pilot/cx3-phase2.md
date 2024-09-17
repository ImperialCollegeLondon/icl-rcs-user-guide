# CX3 Phase 2

!!! warning

    The content will continue to be updated inline with testing on the new platform.

CX3 Phase 2 is a project to migrate the CX3 HPC system to a new server management platform, enabling better management of the operating system and more timely security updates. This upgrade will also enable a significant upgrade to the job scheduler, ensuring better resource management and isolation of jobs when they are running. We are currently migrating hardware with the eventual goal of moving the entirety of the CX3 facility to this new platform.

The purpose of this document is to provide information on how to use CX Phase 2, particularly those differences as compared to the original CX3. Please remember that as this service is in a pre-production phase, services including compute nodes are expected to fail from time to time.

## Access
Access to CX3 Phase 2 is controlled in the same way that it is for the original CX3 - there are no additional access steps. More information on accessing the centrally provided HPC facility can be found at [get-access](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/)

## CX3 Phase 2 Specification

* Compute Nodes
    * 53 * Intel nodes: 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 512 GB RAM per node; 4 GB RAM per core.
    * 36 * AMD Rome with 2x AMD EPYC 7742 64-Core Processor (Rome); 128 cores per node; 1TB per node; 8 GB Ram per core.
* GPU Nodes
    * 7 x Intel nodes, 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1 TB RAM, 8 x [L40S](https://www.nvidia.com/en-us/data-center/l40s/) 48GB GDDR6 GPUs per node.
    * 2 x Intel nodes, 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1 TB RAM, 2 x [A100](https://www.nvidia.com/en-gb/data-center/a100/) 40GB GDDR6 GPUs per node.
    * 1 x Intel node, with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1 TB RAM, 4 x [A40](https://www.nvidia.com/en-gb/data-center/a40/) 48GB GDDR6 GPUs per node.

## Connecting to CX3 Phase 2
To connect to Phase 2 please use login.cx3.hpc.ic.ac.uk which will connect you to one of login nodes. We have both Intel and AMD login nodes so if you want to target a cpu type please check the table below.

| Login node      | CPU Type |
| ----------- | ----------- |
| login-b.cx3.hpc.ic.ac.uk    | AMD EPYC 7742 64-Core Processor (Rome)    |
| login-ai.cx3.hpc.ic.ac.uk   | Intel Xeon Platinum 8358 (Ice Lake)       |
| login-bi.cx3.hpc.ic.ac.uk   | Intel Xeon Platinum 8358 (Ice Lake)       |

For identification the key fingerprints are:

3072 SHA256:nwz0/vwlsKqb8hD+anE34qGKbNZDpo5aUsxzIvv9r3U login.cx3.hpc.ic.ac.uk (RSA)  
256 SHA256:kjRWzuUtzkhdaSzRbQ5pPg4JrkSuGrx1SsfLAfLHv4o login.cx3.hpc.ic.ac.uk (ECDSA)  
256 SHA256:Mddqkum9DwRsvrxoJckFP/aIKuN1+41qIKpU2vuZjIg login.cx3.hpc.ic.ac.uk (ED25519)  

Please see our [Remote Access guide](../../remoteaccess.md) for advice if you have problems connecting the the service. 

## Storage
The RDS is directly connected to the CX3 Phase 2 in the same way as it is for the original CX3, therefore file paths for home and projects remain the same. Your home area on CX3 phase 2 is the same as the original CX3. Please see the following two links for further information:

* [Data Management on HPC](../getting-started/data-management-on-hpc.md)
* [Research Data Store](../../rds/index.md)

## Software

Software installed on the cluster is accessible on CX3 Phase 2 using the module system, as it is for the original CX3. However, the old modules are no longer accessible, only the modules built using EasyBuild are immediately available, along with some binary packages such as Matlab. If you require software from the old module system, please raise a ticket so we can check it's compatibility with the new platform.

### Python and Conda Environments
It is possible to use Conda environments created using the old anaconda3/personal module however the loading menthod is a little different. Load conda with

```console
eval "$(~/anaconda3/bin/conda shell.bash hook)"
```

From then on you can load your conda environments as before, so in a job you would have something like,

```console
eval "$(~/anaconda3/bin/conda shell.bash hook)"
source activate tf2_env
```

where tf2_env is an example of an environment you created with the anaconda3/personal module on the old system. This system works for old environments but when creating new conda environments we recommend installing Miniconda following the instructions below.

#### Miniconda

[Miniconda](https://docs.conda.io/en/latest/miniconda.html) is a minimal installer for conda and only provides those packages needed for setting up your conda environments. It is very quick to install and doesn't provide extra packages from Anaconda that you don't necessarily need. The following instructions are based on those found on the [Miniconda installation page](https://docs.conda.io/en/latest/miniconda-install.html), but have been adapted for our systems.

Create a directory for miniconda and download the latest installer file:

```console
[username@login-b ~]$ mkdir -p ~/miniconda3
[username@login-b ~]$ curl https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -o ~/miniconda3/miniconda.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 98.4M  100 98.4M    0     0   135M      0 --:--:-- --:--:-- --:--:--  135M
```

Then run the installer, setting `~/miniconda3` as the target installation directory:

```console
[username@login-b ~]$ bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
PREFIX=/gpfs/home/username/miniconda3
Unpacking payload ...
 
Installing base environment...
 
 
Downloading and Extracting Packages
 
 
Downloading and Extracting Packages
 
Preparing transaction: done
Executing transaction: done
installation finished.
```
You can then delete the installation script:

```console
[username@login-b ~]$ rm ~/miniconda3/miniconda.sh
```

Our recommended way of using your installed conda is to run the setup script for the bash shell:

```console
[username@login-b ~]$ eval "$(~/miniconda3/bin/conda shell.bash hook)"
(base) [username@login-b ~]$ conda env list
# conda environments:
#
base                     /rds/general/user/username/home/miniconda3
```

Unfortunately due to changes in the Anaconda license users need to manually disable the *defaults* channel and instead use conda-forge. This shouldn't affect most users as most package developers focus on conda-forge anyway. To make this change please run the following:

```console
[username@login-b ~]$ conda config --remove channels defaults
[username@login-b ~]$ conda config --add channels conda-forge
```

If you are not using the base environment, we recommending you disable automatically loading this.

```console
(base) [username@login-b ~]$ conda config --set auto_activate_base false
```

Now whenever you load conda using the eval command only the conda commands will be loaded, Python won't change until you "source activate" the environment you need for your workflow. 
## Job Submission

CX3 Phase 2 uses a new instance of PBS Pro for the management of jobs. This new version has some notable differences in job submission which are described below, please make sure you review this information before submitting jobs to Phase 2.

### Submitting Jobs

The qsub command is used to submit jobs. 

```console
qsub myjob.pbs
```

### Explanation of PBS Directives
The following table provides an explanation of what each directive means in the context of your resource request on CX3 Phase 2. 


| Directive | Description |Default|
| --------- | ----------- |---------|
| `select` | This is the number of compute nodes you wish to use in your job. Most users on CX3 phase 2 will probably just be wanting to use 1 compute node.| 1|
| `ncpus` | This is the number of compute cores to allocate to your job, on each allocated compute node. ncpus=64 would allocate 64 cpus to your job. | 1|
| `mpiprocs` | This is the number of mpi processes you would like to run per allocated compute node. For example, if you want to run 64 mpi processors, you would set mpiprocs=64 |1|
| `ompthreads` | This is the number of openmp threads you want to set, or to be more specific, it sets the OMP_NUM_THREADS environment variable in your job. |1|
| `mem` | This is the amount of memory to allocate to the job per compute node. |1GB|
| `cpu_type` | This is the type of cpu to allocate to your job. This is only required if you need your job to only run on the AMD Rome (cpu_type=rome) or the Intel Icelake (cpu_type=icelake) compute nodes. | Not set|
| `place`	| Sets how the processes for be places on the nodes. The default is "free" which puts them where-ever there is space |free|
| `ngpus`	| This is the number of GPU devices to allocate to your job. | 0|
| `gpu_type` | This is the type of GPU to allocate to your job. For example gpu_type=a100 or gpu_type=a40. | Not set|

The general advice it to set the resources to what is needed for a job but to not over specify. For example, don't set CPU type if your job can run fine or Rome or Icelake. This ensures your job starts as early as possible.

###Job Sizing Guidance

The following queues of jobs are supported:

| Queue | Use Cases | Nodes per job | No. of cores per job<br>(ncpus) | Mem<br>(GB) | Walltime<br>(hrs) |
| ----- | --------- | :-------------: | ---------------------------- | -------- | -------------- |
| small24 | Low core jobs 24h | 1 | 1 - 16 | 1 - 128 | 0 - 24 |
| small72 | Low core jobs 72h | 1 | 1 - 16 | 1 - 128 | 24 - 72 |
| [jupyter](#jupyter) | Queue for JupyterHub jobs* | 1 | 1, 4, 8 | 8, 32, 64 | 2, 4, 8 |
| [jupytergpu](#jupytergpu) | Queue for JupyterHub GPU jobs* | 1 | 4 | 32 | 8 |
| medium24 | Single-node jobs 24h | 1 | 1 - 64 | 1 - 450 | 0 - 24 |
| medium72 | Single-node jobs 72h | 1 | 1 - 64 | 1 - 400 | 24 - 72 |
| large24 | Whole node jobs 24h | 1 | 1 - 128 | 1 - 920 | 0 - 24 |
| large72 | Whole node jobs 72h | 1 | 1 - 128 | 1 - 920 | 24 - 72 |
| largemem72 | Large memory jobs | 1 | 1 - 128 | 921 - 4000 | 0 - 72 |
| gpu72 | Main queue for gpu job* | 1 | 1 - 128 | 1 - 920 | 0 - 72 |
| capability24 | Multi-node jobs 24h | 2 - 4 | 1 - 256 | 1 - 2048 | 0 - 24 |
| capability48 | Multi-node jobs 48h | 2 - 4 | 1 - 256 | 1 - 2048 | 24 - 48 |


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

This would allocate a whole node of an AMD Rome compute node.

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
Multi-node MPI jobs running that use our `intel` modules follow a similar behaviour to that on HX1.
You will need to specify the `mpirun -v6` flag in order to avoid bootstrap errors. 
This is **not** necessary for single-node MPI jobs.
##### 1 compute node 64 mpi tasks per node 200GB RAM per node

Note: You must specify both ncpus and mpiprocs. 

```bash
#!/bin/bash
#PBS -l select=1:ncpus=64:mpiprocs=64:mem=200gb
#PBS -l walltime=00:01:00
```

##### 2 compute nodes with 128 mpi tasks total and 200GB RAM per node

The number in ncpus and mpiprocs is per-node so here we have a total of 128 mpi processes. 

```bash
#!/bin/bash
#PBS -l select=2:ncpus=64:mpiprocs=64:mem=200gb
#PBS -l walltime=00:01:00
```

#### Hybrid OpenMP/MPI Jobs

##### 2 compute nodes 128 cores allocated 32 mpi tasks per node 2 OpenMP threads per task 200GB RAM per node

```bash
#!/bin/bash
#PBS -l select=2:ncpus=64:mpiprocs=32:ompthreads=2:mem=200gb
#PBS -l walltime=00:01:00
```

#### GPU Jobs
##### GPU Specification

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


##### 4 cores 1 GPU 64GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=64gb:ngpus=1
#PBS -l walltime=00:01:00
```

##### 4 cores 1 L40S GPU 64GB RAM

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=64gb:ngpus=1:gpu_type=L40S
#PBS -l walltime=00:01:00
```
