# Issues in running multinode Mpi4py jobs

[Mpi4py](https://mpi4py.readthedocs.io/en/stable/) or MPI for Python provides Python bindings for the Message Passing Interface (MPI) standard, allowing Python applications to exploit multiple processors on workstations, clusters and supercomputers.

In the past, we noticed that some users were having issues in running multinode Python jobs using Mpi4py. This page provides the details of the issue first and then describes how you can resolve it.

## Description of the issue

Let us suppose that you want to run the following sample `Mpi4py` code on multiple nodes. We assume that the file name is `node_name.py`.

```python
# Import modules
import os
from mpi4py import MPI
import socket

# Initialize MPI
comm = MPI.COMM_WORLD
rank = comm.Get_rank()
size = comm.Get_size()
hostname = socket.gethostname()

# Run sample command
print(f"Hello from rank {rank} of {size} on {hostname}")
```

The above code prints the rank, size and hostname of the node on which it is running.

Let us consider that you want to run this code on 2 nodes with 2 processes on each node. The following job script (names as `job.sh`) can be used to run the code.

```bash
#!/bin/bash
#PBS -lselect=2:ncpus=2:mpiprocs=2:mem=20gb
#PBS -lplace=scatter
#PBS -lwalltime=00:10:00


# Load modules for any applications
module load miniforge/3

# Activate the conda environment
eval "$(~/miniforge3/bin/conda shell.bash hook)"
conda activate your_env_name

cd $PBS_O_WORKDIR

mpirun -n 4 python node_name.py
```

First important thing to note in above script is the use of the flag `-lplace=scatter` in the PBS script. This flag is used to ensure that the MPI processes are distributed across the nodes. If this flag is not used, all processes may end up running on a single node, which is not what we want.

Depending on how you installed Mpi4py, you may get the following output when you run the job script:

```bash
--------------------------------------------------------------------------
prterun was unable to find the specified executable file, and therefore did
not launch the job.  This error was first reported for process rank
2; it may have occurred for other processes as well.

NOTE: A common cause for this error is misspelling a prterun command
   line parameter option (remember that prterun interprets the first
   unrecognized command line token as the executable).

   Node:       cx3-13-26
   Executable: ~/miniforge3/envs/your_env_name/bin/python
--------------------------------------------------------------------------
```

The error primarily indicates that the MPI runtime is unable to find the Python executable on the specified node. This can happen if the environment is not properly set up on all nodes. This happens when the `mpi` inside the `conda` environment is used and it was not properly configured for our systems.

To check, if we have the `mpi` installed in the conda environment, we can use the following command:

```bash
$ conda list | grep mpi

# We get the output
# packages in environment at /gpfs/home/jc707/miniforge3/envs/dedalus3_linked_openmpi:
mpi                       1.0                     openmpi              conda-forge
mpi4py                    4.1.0                   py312h39fa25a_100    conda-forge
```

Thus, we can see the `mpi` is indeed there in the conda environment. In most cases, this `mpi` would have been installed because of some dependencies of the packages that you wanted in your conda environment.

To check, if this is the `mpi` being used, we can use the following command:

```bash
$ which mpirun

# We get the output
$ ~/miniforge3/envs/your_env_name/bin/mpirun
```

There are two ways to resolve this issue.

1. To use the `mpi` module that is installed on the system instead of the one in the conda environment.
2. To reinstall the `Mpi4py` package in the conda environment using the `mpi` module that is installed on the system.

## Resolution without reinstalling Mpi4py

The easiest way to resolve this issue is to use a suitable `mpi` module that is available on the system. You need to make sure that you first load and activate `conda` environment and then you load the `mpi` module.

The order is important here as it will ensure that `mpi` library from the system module takes precedence over the one in the conda environment.

For example, you can modify the job script as follows:

```bash
# Load modules for any applications
module load miniforge/3

# Activate the conda environment
eval "$(~/miniforge3/bin/conda shell.bash hook)"
conda activate your_env_name

# Add the mpi module here
module load OpenMPI/4.1.6-GCC-13.2.0
```

Now, if you type the command `which mpirun`, you should see the path to the `mpirun` executable from the `OpenMPI` module that you just loaded.

```bash
$ which mpirun

# will give
$ /sw-eb/software/OpenMPI/4.1.6-GCC-13.2.0/bin/mpirun
```

On running the code, you will now get the expected output without any errors.

```bash
Hello from rank 2 of 4 on cx3-13-26
Hello from rank 3 of 4 on cx3-13-26
Hello from rank 1 of 4 on cx3-13-25
Hello from rank 0 of 4 on cx3-13-25
```

## Resolution by reinstalling Mpi4py

The recommended way to install Mpi4py on our system is to use the `mpi` module that is installed on the system. This ensures that the Mpi4py package is properly configured to work with the MPI runtime on our systems.

Below is a set of commands that can be used to reinstall `Mpi4py` correctly.

```bash
# Load the mpi module
$ module load OpenMPI/4.1.6-GCC-13.2.0

# Create a new conda environment
$ module load miniforge/3
$ eval "$(~/miniforge3/bin/conda shell.bash hook)"

$ conda create -n your_env_name
$ conda activate your_env_name
$ conda install pip
$ pip install mpi4py
```

Once you install `Mpi4py` using the above commands, you can run the job script as before without any issues.
