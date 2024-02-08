# EasyBuild

We are now using the software installation system called [EasyBuild](https://easybuild.io) to manage the installation of user software requests. Key advantages of using this software management tool are that we can now provide a method to manage production vs development versions of software applications, provide versions optimised for the different types of hardware available within RCS systems, and provide users themselves with the ability to manage re-deployable versions of their own software.

Use of EasyBuild has many advantages including making it quicker and easier to install applications that are already part of the EasyBuild system. At present over 2900 applications are already supported through Easybuild and as part of the broader community using EasyBuild application installs have already been peer-reviewed and tested across machines at numerous other HPC sites.

## EasyBuild Software Stacks
Software is now presented via two stacks:

* [Development](#using-the-development-software-stack)
* [Production](#using-the-production-software-stack)

The development stack is for software which is currently not available directly from EasyBuild and is a generic build. It should work on the entire cluster, but no guarantee can be given here. This software stack might change on short notice due to required rebuilds. The production stack is for software which is already available as an EasyBuild package and which has been built to support specific CPU architectures thereby providing better performance than the generic builds. The following two sections provide examples of how to use the two software stacks.

## Using the Development Software Stack

### On the Login Nodes

The EasyBuild development software stack may be accessed on the login nodes by first loading the 'tools/dev' module and then the corresponding program module (see the Applications for more advice on using the module system). The following examples demonstrates how to use the development software stack on the login nodes (please remember when loading modules on the login node, it is forbidden to run computational workloads):

Loading the development software stack on the login nodes and viewing available modules.

```console
[username@login ~]$ module load tools/dev
[username@login ~]$ module avail
-------------------------------------- /apps/sw-eb/modules/all ---------------------------------------
ADIOS/1.13.1-foss-2020a-Python-3.8.2                     SciPy-bundle/2020.11-foss-2020b
ADIOS/20210804-foss-2020a-Python-3.8.2                   SciPy-bundle/2021.05-foss-2021a
ambit/733c529-foss-2021b-psi4                            SciPy-bundle/2021.05-intel-2021a
AMPL-MP/3.1.0-GCCcore-10.3.0                             SciPy-bundle/2021.10-foss-2021b
ant/1.10.9-Java-11                                       SciPy-bundle/2022.05-foss-2022a
ant/1.10.11-Java-11                                      SCOTCH/6.0.9-gompi-2020a
ANTLR/2.7.7-GCCcore-11.3.0-Java-11                       SCOTCH/6.1.0-gompi-2020b
archspec/0.1.2-GCCcore-10.2.0                            SCOTCH/6.1.0-gompi-2021a
archspec/0.1.2-GCCcore-10.3.0                            SCOTCH/6.1.2-gompi-2021b
archspec/0.1.3-GCCcore-11.2.0                            SCOTCH/7.0.1-gompi-2022a
archspec/0.1.4-GCCcore-11.3.0                            SCOTCH/7.0.1-iimpi-2022a
...
```

Loading the development software stack on the login nodes and loading the GROMACS/2021.5-foss-2021b module.

```console
[username@login ~]$ module load tools/dev
[username@login ~]$ module load GROMACS/2021.5-foss-2021b
Loading GROMACS/2021.5-foss-2021b
  Loading requirement: GCCcore/11.2.0 zlib/1.2.11-GCCcore-11.2.0 binutils/2.37-GCCcore-11.2.0
    GCC/11.2.0 numactl/2.0.14-GCCcore-11.2.0 XZ/5.2.5-GCCcore-11.2.0 libxml2/2.9.10-GCCcore-11.2.0
    libpciaccess/0.16-GCCcore-11.2.0 hwloc/2.5.0-GCCcore-11.2.0 OpenSSL/1.1
    libevent/2.1.12-GCCcore-11.2.0 UCX/1.11.2-GCCcore-11.2.0 libfabric/1.13.2-GCCcore-11.2.0
    PMIx/4.1.0-GCCcore-11.2.0 OpenMPI/4.1.1-GCC-11.2.0 OpenBLAS/0.3.18-GCC-11.2.0
    FlexiBLAS/3.0.4-GCC-11.2.0 gompi/2021b FFTW/3.3.10-gompi-2021b ScaLAPACK/2.1.0-gompi-2021b-fb
    foss/2021b bzip2/1.0.8-GCCcore-11.2.0 ncurses/6.2-GCCcore-11.2.0 libreadline/8.1-GCCcore-11.2.0
    Tcl/8.6.11-GCCcore-11.2.0 SQLite/3.36-GCCcore-11.2.0 GMP/6.2.1-GCCcore-11.2.0
    libffi/3.4.2-GCCcore-11.2.0 Python/3.9.6-GCCcore-11.2.0 pybind11/2.7.1-GCCcore-11.2.0
    SciPy-bundle/2021.10-foss-2021b networkx/2.6.3-foss-2021b Loading GROMACS/2021.5-foss-2021b
```

Note: As evident from the example above, apart from the "gateway" module "tools/dev" only the module for the software you want to use is required to be loaded. Any other module which is required will be loaded now automagically. This is different from the old system where you might need to load a compiler and MPI module. The new system that does not required this as the requirements for that software will be loaded automatically.
Also, if you want to use two different programs at the same time, say A and B, you need to make sure you are using the same toolchain. In the example above, the toolchain would be foss and the version would be 2021b. Thus, you only could use B/1.2.3-foss-2021b but not B/1.2.3-foss-2022a together with GROMACS/2021.5-foss-2021b example from above!

### Within a Batch Job

Using the development software stack within a batch job is the same as for using it on the login node. For example to load GROMACS 2021.5 from the development stack and use within a batch you, you could do the following:

Using GROMACS 2021.5 from the development software stack in a batch job.

```console
#PBS -lwalltime=01:00:00
#PBS -lselect=1:ncpus=16:mem=16gb

module purge
module load tools/dev
module load GROMACS/2021.5-foss-2021b

mpirun gmx_mpi <your options>
```

## Using the Production Software Stack

### On the Login Nodes

The production software stack cannot be directly loaded on the login nodes. However, it IS possible to "view" which software has been installed and is therefore available on the compute nodes. To do this, you need to load the 'tools/prod-headnode' module and search for the software you require using the module commands (as described on the Applications page).  The following examples demonstrates how to use the production software stack on the login nodes (please remember when loading modules on the login node, it is forbidden to run computational workloads):

Loading the production software stack on the login nodes and viewing available modules.

```console
[username@login ~]$ module load tools/prod-headnode
[username@login ~]$ module avail
------------------------------ /rds/easybuild/haswell/apps/modules/all -------------------------------
ANTLR/2.7.7-GCCcore-11.3.0-Java-11                       SciPy-bundle/2020.11-foss-2020b
archspec/0.1.2-GCCcore-10.2.0                            SciPy-bundle/2021.05-foss-2021a
archspec/0.1.2-GCCcore-10.3.0                            SciPy-bundle/2021.10-foss-2021b
archspec/0.1.3-GCCcore-11.2.0                            SciPy-bundle/2022.05-foss-2022a
archspec/0.1.4-GCCcore-11.3.0                            SCOTCH/6.0.9-gompi-2020a
arpack-ng/3.8.0-foss-2021b                               SCOTCH/6.1.0-gompi-2020b
astropy/5.1.1-foss-2022a                                 SCOTCH/6.1.0-gompi-2021a
ATK/2.38.0-GCCcore-11.3.0                                SCOTCH/7.0.1-gompi-2022a
Autoconf/2.69-GCCcore-8.3.0                              SCOTCH/7.0.1-iimpi-2022a
Autoconf/2.69-GCCcore-9.3.0                              snappy/1.1.8-GCCcore-9.3.0 
...
```

Loading the development software stack on the login nodes and loading the GROMACS/2021.5-foss-2021b module.

```console
[username@login ~]$ module load tools/prod-headnode
[username@login ~]$ module load GROMACS/2021.5-foss-2021b
Loading GROMACS/2021.5-foss-2021b
  Loading requirement: GCCcore/11.2.0 zlib/1.2.11-GCCcore-11.2.0 binutils/2.37-GCCcore-11.2.0
    GCC/11.2.0 numactl/2.0.14-GCCcore-11.2.0 XZ/5.2.5-GCCcore-11.2.0 libxml2/2.9.10-GCCcore-11.2.0
    libpciaccess/0.16-GCCcore-11.2.0 hwloc/2.5.0-GCCcore-11.2.0 OpenSSL/1.1
    libevent/2.1.12-GCCcore-11.2.0 UCX/1.11.2-GCCcore-11.2.0 libfabric/1.13.2-GCCcore-11.2.0
    PMIx/4.1.0-GCCcore-11.2.0 OpenMPI/4.1.1-GCC-11.2.0 OpenBLAS/0.3.18-GCC-11.2.0
    FlexiBLAS/3.0.4-GCC-11.2.0 gompi/2021b FFTW/3.3.10-gompi-2021b ScaLAPACK/2.1.0-gompi-2021b-fb
    foss/2021b bzip2/1.0.8-GCCcore-11.2.0 ncurses/6.2-GCCcore-11.2.0 libreadline/8.1-GCCcore-11.2.0
    Tcl/8.6.11-GCCcore-11.2.0 SQLite/3.36-GCCcore-11.2.0 GMP/6.2.1-GCCcore-11.2.0
    libffi/3.4.2-GCCcore-11.2.0 Python/3.9.6-GCCcore-11.2.0 pybind11/2.7.1-GCCcore-11.2.0
    SciPy-bundle/2021.10-foss-2021b networkx/2.6.3-foss-2021b Loading GROMACS/2021.5-foss-2021b
```

Note that while you can view the modules from the production software stack on the login node, you cannot actually run them. For example, after loading GROMACS from the production software stack, you will not be able to run the GROMACS gmx application.

```console
[username@login ~]$ gmx
-bash: gmx: command not found
```

### Within a Batch Job

To use the production software stack within a batch job, you need to load the tools/prod module rather than the tools/prod-headnode module.

For example to load GROMACS 2021.5 from the production software stack and use within a batch you, you could do the following:

Using GROMACS 2021.5 from the production software stack in a batch job.

```bash
#PBS -lwalltime=01:00:00
#PBS -lselect=1:ncpus=16:mem=16gb

module purge
module load tools/prod
module load GROMACS/2021.5-foss-2021b

mpirun gmx_mpi <your options>
```

## Using the GPU Software Stack

As part of the architecture specific installation, we are also building software specifically for our GPU nodes.

### On the login nodes

The production GPU software stack cannot be directly loaded on the login nodes. However, it IS possible to "view" which software has been installed and is therefore available on the compute nodes. To do this, you need to load the tools/gpu-headnode module and search for the software you require using the module commands (as described on the [Applications](./index.md) page).  The following examples demonstrates how to use the production GPU software stack on the login nodes; please remember when loading modules on the login node, it is forbidden to run computational workloads:

Loading the production GPU software stack on the login nodes and viewing available modules.

```console
[username@login ~]$ module load tools/gpu-headnode
[username@login ~]$ module avail
------------------------------ /rds/easybuild/skylake/apps/modules/all -------------------------------
AlphaFold/2.0.0-fosscuda-2020b                            tqdm/4.61.2-GCCcore-10.3.0
AlphaFold/2.1.2-foss-2021a-CUDA-11.3.1                    tqdm/4.62.3-GCCcore-11.2.0
ant/1.10.9-Java-11                                        typing-extensions/3.7.4.3-GCCcore-10.2.0
ant/1.10.11-Java-11                                       typing-extensions/3.10.0.0-GCCcore-10.3.0
at-spi2-atk/2.38.0-GCCcore-10.3.0                         typing-extensions/3.10.0.2-GCCcore-11.2.0
at-spi2-atk/2.38.0-GCCcore-11.2.0                         UCC/1.0.0-GCCcore-11.3.0
at-spi2-core/2.40.2-GCCcore-10.3.0                        UCX-CUDA/1.10.0-GCCcore-10.3.0-CUDA-11.3.1
at-spi2-core/2.40.3-GCCcore-11.2.0                        UCX-CUDA/1.11.2-GCCcore-11.2.0-CUDA-11.4.1
ATK/2.36.0-GCCcore-10.3.0                                 UCX-CUDA/1.12.1-GCCcore-11.3.0-CUDA-11.7.0
ATK/2.36.0-GCCcore-11.2.0                                 UCX/1.9.0-GCCcore-10.2.0
Autoconf/2.69-GCCcore-7.3.0                               UCX/1.9.0-GCCcore-10.2.0-CUDA-11.1.1
...
```

Loading the development software stack on the login nodes and loading the GROMACS/2021.5-foss-2021b module.

```console
[username@login ~]$ module load tools/gpu-headnode
[username@login ~]$ module load GROMACS/2021.3-foss-2021a-CUDA-11.3.1
Loading GROMACS/2021.3-foss-2021a-CUDA-11.3.1
  Loading requirement: GCCcore/10.3.0 zlib/1.2.11-GCCcore-10.3.0 binutils/2.36.1-GCCcore-10.3.0
    GCC/10.3.0 numactl/2.0.14-GCCcore-10.3.0 XZ/5.2.5-GCCcore-10.3.0 libxml2/2.9.10-GCCcore-10.3.0
    libpciaccess/0.16-GCCcore-10.3.0 hwloc/2.4.1-GCCcore-10.3.0 OpenSSL/1.1
    libevent/2.1.12-GCCcore-10.3.0 UCX/1.10.0-GCCcore-10.3.0 libfabric/1.12.1-GCCcore-10.3.0
    PMIx/3.2.3-GCCcore-10.3.0 OpenMPI/4.1.1-GCC-10.3.0 OpenBLAS/0.3.15-GCC-10.3.0
    FlexiBLAS/3.0.4-GCC-10.3.0 gompi/2021a FFTW/3.3.9-gompi-2021a ScaLAPACK/2.1.0-gompi-2021a-fb
    foss/2021a bzip2/1.0.8-GCCcore-10.3.0 ncurses/6.2-GCCcore-10.3.0 libreadline/8.1-GCCcore-10.3.0
    Tcl/8.6.11-GCCcore-10.3.0 SQLite/3.35.4-GCCcore-10.3.0 GMP/6.2.1-GCCcore-10.3.0
    libffi/3.3-GCCcore-10.3.0 Python/3.9.5-GCCcore-10.3.0 pybind11/2.6.2-GCCcore-10.3.0
    SciPy-bundle/2021.05-foss-2021a networkx/2.5.1-foss-2021a CUDA/11.3.1 GDRCopy/2.2-GCCcore-10.3.0
    UCX-CUDA/1.10.0-GCCcore-10.3.0-CUDA-11.3.1
```

Note that while you can view the modules from the production software stack on the login node, you cannot actually run them. For example, after loading GROMACS from the production software stack, you will not be able to run the GROMACS gmx application.

```console
[username@login ~]$ gmx
-bash: gmx: command not found
```

### Within a batch job

To use the production software stack within a batch job, you need to load the tools/prod module rather than the tools/gpu-headnode module.

For example to load GROMACS 2021.3 (with CUDA acceleration) from the production GPU software stack and use within a batch you, you could do the following:

Using production GPU modules within a batch script

```bash
#PBS -lwalltime=01:00:00
#PBS -l select=1:ncpus=4:mem=24gb:ngpus=1:gpu_type=RTX6000

module purge
module load tools/prod
module load GROMACS/2021.3-foss-2021a-CUDA-11.3.1

gmx <your options>
```

More information on running GPU jobs may be found on our [GPU Jobs page](../queues/gpu-jobs.md)

## User Software Development
In order to make it easier for those users who are either developing software, or need to quickly install some software to test, we are now offering a development module: 'tools/eb-dev'.

This allows you to install software with EasyBuild in your own user space, so that you can use a locally modified version of software, or set up a reproducible build environment when working on your own software development projects. The latter will allow you to change your build environment without going through Anaconda or any other virtual environment.
If you only want to switch compilers etc, you can make use of the already installed "buildenv" modules which will give you convenient access to various versions of both GCC and Intel compilers, next to MPI access.

Note: EasyBuild is quite prescriptive, so if you are selecting a specific tool-chain you will get a specific version of the compiler, MPI etc. It is not a mix-and-match approach.
Note: Remember we got a heterogeneous cluster regarding the various CPUs we are using (AMD, and Intel). Care needs to be taken when building software from source to make sure it will run on the cluster. Please come to one of our workshops if you are  doing software development seriously.

We will be offering hands-on workshops throughout the year to explain the benefits of using EasyBuild for your software development project for example, or simply how to make the best use of the new and exciting software stack. These workshops will be announced via the usual channels.