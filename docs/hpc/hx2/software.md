## Software

This section will explain how to access software that has been centrally installed on HX2.

Please make sure any software you run on HX2 has been optimised for the hardware. Ideally use software provided by us (which has already been optimised) or otherwise please make sure to use relevant optimisation flags when compiling your own software. Please avoid simply copying binaries from other systems such as CX3, unless the software is commercial and/or only the binary is available.

### Loading Applications

Loading modules/applications on HX2 is similar to that on CX3 except it is not necessary to load the "production" modules (`tools/prod`). Please refer to our main [Loading Applications](../applications/index.md) page for advice on how to load modules on HX1.

### EasyBuild

Most of the software installed on HX2 is done so using the EasyBuild software installation system and will have been optimised for the hardware. Please see our [EasyBuild](../applications/easybuild.md) page for more information.

### Python and Conda Environments

#### Installing miniforge

[Miniforge](https://github.com/conda-forge/miniforge) is a minimal installer for conda that is tailored to use the conda-forge channel by default. It installs both the conda and mamba tools for managing your environments.

The following instructions are based on those found on the Miniforge repository by have been adapted for use on HX1.

```console
[username@hx2a04login01 ~]$ curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 89.3M  100 89.3M    0     0  93.1M      0 --:--:-- --:--:-- --:--:--  194M
```

You should end up with a file called `Miniforge3-Linux-x86_64.sh`. You can then run the installer with:

```console
[username@hx2a04login01 ~]$ bash Miniforge3-Linux-x86_64.sh

Welcome to Miniforge3 25.3.0-3

In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>>
```

After accepting the license, you will be asked to confirm where to install Miniforge3. The default location of `miniforge3` in your home directory is fine for most circumstances.

Once the files have finished unpacking, you will be asked:

```console
Do you wish to update your shell profile to automatically initialize conda?
This will activate conda on startup and change the command prompt when activated.
If you'd prefer that conda's base environment not be activated on startup,
   run the following command when conda is activated:

conda config --set auto_activate_base false

You can undo this by running `conda init --reverse $SHELL`? [yes|no]
[no] >>>
```

We strongly advise that you do not update your shell profile i.e. respond with "no".

You can now enable miniforge in your environment by running the shell hook:

```console
[username@hx2a04login01 ~]$ eval "$(/hx2-weka/home/username/miniforge3/bin/conda shell.bash hook)"
(base) [username@hx2a04login01 ~]$
```

You can then use the conda commands as usual. Please see our [Conda](../applications/guides/conda.md) page for more information on using conda.

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

#### gpu/h200

There is an additional limit of 12 GPU's total per user on the a100 queue to allow for fair usage of the GPUs.

### Example Slurm Jobs
#### MPI Jobs


#### Hybrid OpenMP/MPI Jobs

### MPI Distribution Specific Information

#### Intel MPI

#### OpenMPI

### GPU Jobs
#### GPU Specification


#### Multi-node GPU Jobs

#### Example GPU Jobs

##### MPI GPU Jobs (single node)

## Known Issues
