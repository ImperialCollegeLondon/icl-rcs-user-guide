# HX6


!!! warning

    Please be aware that HX6 is currently a pre-production service. You should expect occasional service interruptions, which may occur with limited or no prior notice.


HX6 or HeX6, also referred to as Imperial Novel Architecture, or INovArc, is an experimental HPC platform at Imperial. It is intended as a playground for exploring novel computing architectures and for developing, testing and evaluating workloads on emerging hardware.


The initial HX6 configuration combines AMD EPYC processors with eight AMD Instinct MI300X accelerators. The platform currently consists of one login node, one compute node and a Slurm workload manager.

## Cluster Specification

As of 12/10/2026, HX6 comprises of the following specs:

HX6 runs Red Hat Enterprise Linux 9.6.


| Component | Specification |
| --- | --- |
| Processors | 2 × AMD EPYC 9534 |
| CPU cores | 64 cores per socket; 128 cores in total |
| GPUs | 8 × AMD Instinct MI300X |
| GPU memory | 192 GB per GPU; 1,536 GB in total |
| System memory | 2.2Tb |
| Local storage | ~4Tb NVMe SSD |


!!! info

    Currently, HX6 has a single compute node. Jobs cannot run across multiple compute nodes.


## Access

Users must already be registered for the Imperial RCS HPC service before requesting access to HX6. See the [RCS HPC access page](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) for information about registering for the HPC service.

Once registered, request access to HX6 using the [ServiceDesk Ask form](https://servicemgt.service-now.com/ask?id=sc_cat_item&sys_id=470ee82a1b9df9101533a8a4bd4bcbd7&sysparm_category=52a4a8f21be62110557837b5464bcbd2).

### Connecting to HX6

There is one login node for HX6 which is accessible over SSH. This can be accessed using the `login.hx6.hpc.ic.ac.uk` hostname.

If you see a message asking you to confirm the host key, the fingerprint shown in the message should match the one given below:

- SHA256:ApFQ9E6O+/jWpqdXnLtFR5i/4iY2ve251euKqN0OISQ (ED25519)

Due to security requirements, key-based authentication is disabled on the login node. Users must log in using their Imperial username and password:

```console
$ ssh username@login.hx6.hpc.ic.ac.uk
```

The login node should be used for activities such as editing files, preparing job scripts and submitting or monitoring jobs. Computationally intensive work must be submitted to the Slurm scheduler.


!!! warning

    Access to all RCS systems and services requires the use of zscaler when working off campus.


## Storage

HX6 currently provides two storage areas:

| Storage area | Total capacity |
| --- | ---: |
| User home directories | 1 TB |
| Project space | 3 TB |

These capacities are shared across all HX6 users. They are not per-user allocations.

There are currently no enforced storage quotas on HX6. Users must therefore consider the available shared capacity when planning their storage requirements and be mindful of their impact on other users.

Please remove data that is no longer required and avoid using HX6 for unnecessary or long-term storage. If you are unsure whether your planned storage usage is appropriate, contact RCS before transferring or generating the data.


## Software

HX6 does not provide a central or globally managed software stack. Users are responsible for bringing, building and maintaining the software required by their workloads.

Software must be installed in a user-writable location, normally within the user's home directory. Users can:

- compile applications from source;
- create user-managed [Conda](https://conda-forge.org/download/) environments; or
- use [EasyBuild](https://docs.easybuild.io/en/latest/Installation.html) to build and install software in their home directory.

Users do not have administrative privileges and must not assume that packages can be installed system-wide.

Software should be built for the AMD CPU architecture and, where applicable, the AMD Instinct MI300X GPUs. Avoid copying binaries from another HPC platform unless they are known to be compatible with HX6.

!!! warning

    Do not run compute-intensive applications or large software builds directly on the login node. Submit this work to Slurm.

## Job Submission


HX6 uses the [Slurm workload manager](https://slurm.schedmd.com/) for job submission and resource allocation.

HX6 has one partition, named `gpu`. This is the default partition and contains the single HX6 compute node.

| Partition | Nodes | CPU cores per node | GPUs per node | Maximum walltime |
| --- | ---: | ---: | ---: | ---: |
| `gpu` | 1 | 128 | 8 × AMD Instinct MI300X | 48 hours |

Jobs cannot run across multiple compute nodes.

### Key commands

| Command | Description |
| --- | --- |
| `sbatch job.slurm` | Submit a batch job |
| `squeue` | Display all jobs in the queue |
| `squeue -u "$USER"` | Display your jobs |
| `sinfo -p gpu` | Display the state of the `gpu` partition |
| `scontrol show job JOB_ID` | Display detailed information about a job |
| `scancel JOB_ID` | Cancel a job |

### Requesting GPUs

The MI300X GPUs are configured as typed Slurm generic resources. Request them using:

```bash
#SBATCH --gres=gpu:mi300x:NUMBER_OF_GPUS
```

For example, request one MI300X GPU with:

```bash
#SBATCH --gres=gpu:mi300x:1
```

Up to eight GPUs are available on the compute node. Request only the number required by your workflow.

Slurm requires typed GPU requests to remain consistent throughout a job. If an `srun` command specifies GPU resources explicitly, it must also use the `mi300x` type. See the [Slurm generic resource documentation](https://slurm.schedmd.com/gres.html) for details.

### Basic GPU job script

The following example requests one MI300X GPU, eight CPU cores, 32 GB of system memory and a walltime of 30 minutes:

```bash
#!/bin/bash
#SBATCH --job-name=example-gpu-job
#SBATCH --partition=gpu
#SBATCH --time=00:30:00
#SBATCH --nodes=1
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=8
#SBATCH --mem=32G
#SBATCH --gres=gpu:mi300x:1
#SBATCH --output=%x-%j.out

cd $HOME/my_project

srun ./my_program
```

Save the script as `example.slurm` and submit it using:

```console
$ sbatch example.slurm
Submitted batch job 12345
```

Monitor the job using:

```console
$ squeue -u "$USER"
```

Cancel it, if required, using the job ID returned by `sbatch`:

```console
$ scancel 12345
```

Standard output and standard error from the example are written to a file named using the job name and job ID, such as:

```text
example-gpu-job-12345.out
```

### Walltime

The maximum permitted walltime is 48 hours. Users should request a realistic walltime for each job rather than relying on the partition default.

For example, request the maximum walltime with:

```bash
#SBATCH --time=2-00:00:00
```

!!! info

   The standard maximum walltime on the `gpu` partition is 48 hours. This limit is there to allow for fair share and prevent individual jobs from monopolising the compute resource for extended periods.
   Jobs requiring more than 48 hours may be permitted by prior arrangement. Raise a support ticket with the RCS team before submitting the job so that its requirements and impact on other users can be discussed.
