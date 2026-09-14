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

The final partition/queue layout is still being developed. Note that on standard compute nodes, jobs are limited to 142 out of the 144 available cores; 2 cores are set aside to support the Weka file system and operating system. This is increased to 4 cores on GPU nodes, so a maximum of 60 cores can be requested.

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

