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
#SBATCH --partition=small

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

!!! info
    When you submit jobs on HX2, you must specify a partition. For example:

    ```bash
    #SBATCH --partition=large
    ```

The final partition layout is still being developed. Note that on standard compute nodes, jobs are limited to 142 out of the 144 available cores; 2 cores are set aside to support the Weka file system and operating system. This is increased to 4 cores on GPU nodes, so a maximum of 60 cores can be requested.

The following partitions are supported:

| Queue | Use Cases | Nodes per job | No. of cores per node<br>(ncpus) | Mem per node<br>(GB) | Walltime<br>(hrs) |
| ----- | --------- | :-------------: | ---------------------------- | -------- | -------------- |
| [small](#small) | Low core jobs | 1 | 1 - 16 | 1 - 128 | 0 - 72 |
| [medium](#medium) | Single-node jobs | 1 | 17 - 72 | 1 - 1000 | 0 - 72 |
| [large](#large) | Whole node jobs | 1 | 73 - 142 | 1 - 2200 | 0 - 72 |
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
[user@login ~]$ srun --nodes=1  --ntasks-per-node=1 --cpus-per-task=16 --mem=32GB --time=08:00:00 --gres=gpu:1 --partition=interactive-gpu --pty bash
```

!!! warning 
	You can only run one interactive job at a time. This means you can either run an interactive CPU job, or an interactive GPU job at once, not at the same time.

#### gpu

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


### HX2 cluster Limits

To ensure fair access to resources for all researchers there are a couple of limits that apply across all queues:

* Maximum of 1040 CPUs running across all jobs, including interactive jobs
* Maximum of 12 GPUs running across all jobs, including interactive jobs

### Explanation of Slurm Directives
The following table provides an explanation of what each directive means in the context of your resource request on HX2.

| Directive | Description |Default|
| --------- | ----------- |---------|
| `nodes` | This is the number of compute nodes you wish to use in your job. Most users on HX2 will probably just be wanting to use 1 compute node.| 1|
| `ntasks‑per‑node` | This is the number of tasks or processes created per node. These usually relate to MPI ranks per node. | 1|
| `cpus‑per‑task` | This is the number of CPU cores allocated per above task. Effectively, this is the number of threads per task/MPI rank.  | 1|
| `mem` | This is the amount of memory to allocate to the job, per compute node. |1GB per core|
| `gres`       | This flag can be used to request [GPU's](#gpu) and [scratch](./storage.md#local-node-storage) space. | None|
| `partition`       | The partition you want to submit your job to. A list of available options can be viewed in the [job sizing guidance](#job-sizing-guidance) section.  | small|


The general advice it to set the resources to what is needed for a job but to not over specify. This ensures your job starts as early as possible.

### Example Slurm Jobs

#### Single Core Jobs

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
