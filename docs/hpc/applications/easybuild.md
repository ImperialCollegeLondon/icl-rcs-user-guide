# EasyBuild

We are now using the software installation system called [EasyBuild](https://easybuild.io) to manage the installation of user software requests. Key advantages of using this software management tool are that we can now provide a method to manage production vs development versions of software applications, provide versions optimised for the different types of hardware available within RCS systems, and provide users themselves with the ability to manage re-deployable versions of their own software.

Use of EasyBuild has many advantages including making it quicker and easier to install applications that are already part of the EasyBuild system. At present over 3670 applications are already supported through Easybuild and as part of the broader community using EasyBuild application installs have already been peer-reviewed and tested across machines at numerous other HPC sites.

## EasyBuild Software Stacks
Software is now presented via two stacks:

* [Production](#using-the-production-software-stack)
* [Development](#using-the-development-software-stack)

The development stack is for software which is currently not available directly from EasyBuild and is a generic build. It should work on the entire cluster, but no guarantee can be given here. This software stack might change on short notice due to required rebuilds. The production stack is for software which is already available as an EasyBuild package and which has been built to support specific CPU architectures thereby providing better performance than the generic builds. The following two sections provide examples of how to use the two software stacks.

## Using the Production Software Stack

### On the Login Nodes

The production software stack is loaded per default on the login nodes and compute nodes. The following examples demonstrates how to use the production software stack on the login nodes (please remember when loading modules on the login node, it is forbidden to run computational workloads):

Loading the production software stack on the login nodes and viewing available modules.

```console
[username@login ~]$ module avail
------------------------------ /sw-eb/modules/all -------------------------------
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

As evident, this is a rather long detailed list. It is often better just to have an **overview** of the modules which are available.

```console
[username@login ~]$ ml ov
------------------------------ /sw-eb/modules/all -------------------------------
AFNI                  (1)    GTK2            (1)
ANTs                  (1)    GTK3            (4)
ATK                   (4)    GTS             (1)
Abseil                (2)    Gaussian        (1)
Armadillo             (2)    Gdk-Pixbuf      (4)
Arrow                 (4)    Ghostscript     (4)
Autoconf              (8)    Graphviz        (1)
...
```

Note the change from using `module` to using the Lmod specific `ml` command. Lmod makes searching for particular software easier as well:

```console
[username@login ~]$ ml spider groma
----------------------
  GROMACS:
----------------------
    Description:
      GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the Newtonian
      equations of motion for systems with hundreds to millions of particles. This is a CPU
      only build, containing both MPI and threadMPI binaries for both single and double
      precision. It also contains the gmxapi extension for the single precision MPI build. 

     Versions:
        GROMACS/2021.3-foss-2021a-PLUMED-2.7.2
        GROMACS/2021.3-foss-2021a
        GROMACS/2021.5-foss-2021b-PLUMED-2.8.0
        GROMACS/2021.5-foss-2021b
        GROMACS/2023.1-foss-2022a
        GROMACS/2023.3-foss-2023a
        GROMACS/2024.1-foss-2023b
```

This conveniently shows all the installed modules starting with `groma` . Note it is case *insensitive* and even shorter stings can be searched. 

More information about a specific module can be obtained too.

```console
 [username@login ~]$ ml spider GROMACS/2024.1-foss-2023b
----------------------
  GROMACS: GROMACS/2024.1-foss-2023b
----------------------
    Description:
      GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the Newtonian
      equations of motion for systems with hundreds to millions of particles. This is a CPU
      only build, containing both MPI and threadMPI binaries for both single and double
      precision. It also contains the gmxapi extension for the single precision MPI build. 


    You will need to load all module(s) on any one of the lines below before the 
    "GROMACS/2024.1-foss-2023b" module is available to load.

      tools/prod
 
    Help:
      
      Description
      ===========
      GROMACS is a versatile package to perform molecular dynamics, i.e. simulate the
      Newtonian equations of motion for systems with hundreds to millions of
      particles.
      
      This is a CPU only build, containing both MPI and threadMPI binaries
      for both single and double precision.
      
      It also contains the gmxapi extension for the single precision MPI build.
          
      More information
      ================
       - Homepage: https://www.gromacs.org
          
      Included extensions
      ===================
      gmxapi-0.4.2

```

Again, note it is also listing the *extensions* which are included in the build. That is particularly interesting if and when you looking for say **Python**, **R** or other software packages which are using extensions. 

Loading the production software stack on the login nodes and loading the GROMACS/2024.1-foss-2023b module.

```console
[username@login ~]$ ml GROMACS/2024.1-foss-2023b
```

Note that it does not print out which modules are loaded, per default `ml` *silently* loads the required modules. Also, only the module of the software you want to use is required, all dependencies are automagically resolved. Furthermore, the `-foss-2023b` is what is called a *toolchain*. You only can load software within the same toolchain hierarchy, you cannot mix-and-match. See [here](https://docs.easybuild.io/common-toolchains/#toolchains_diagram) for more information.

To view a full list of currently loaded modules, you would do:

```console
[username@login ~]$ ml
Currently Loaded Modules:
  1) tools/prod                        12) libevent/2.1.12-GCCcore-13.2.0   23) ScaLAPACK/2.2.0-gompi-2023b-fb  34) cryptography/41.0.5-GCCcore-13.2.0
  2) GCCcore/13.2.0                    13) UCX/1.15.0-GCCcore-13.2.0        24) foss/2023b                      35) virtualenv/20.24.6-GCCcore-13.2.0
  3) zlib/1.2.13-GCCcore-13.2.0        14) libfabric/1.19.0-GCCcore-13.2.0  25) bzip2/1.0.8-GCCcore-13.2.0      36) Python-bundle-PyPI/2023.10-GCCcore-13.2.0
  4) binutils/2.40-GCCcore-13.2.0      15) PMIx/4.2.6-GCCcore-13.2.0        26) ncurses/6.4-GCCcore-13.2.0      37) pybind11/2.11.1-GCCcore-13.2.0
  5) GCC/13.2.0                        16) UCC/1.2.0-GCCcore-13.2.0         27) libreadline/8.2-GCCcore-13.2.0  38) SciPy-bundle/2023.11-gfbf-2023b
  6) numactl/2.0.16-GCCcore-13.2.0     17) OpenMPI/4.1.6-GCC-13.2.0         28) Tcl/8.6.13-GCCcore-13.2.0       39) networkx/3.2.1-gfbf-2023b
  7) XZ/5.4.4-GCCcore-13.2.0           18) OpenBLAS/0.3.24-GCC-13.2.0       29) SQLite/3.43.1-GCCcore-13.2.0    40) mpi4py/3.1.5-gompi-2023b
  8) libxml2/2.11.5-GCCcore-13.2.0     19) FlexiBLAS/3.3.1-GCC-13.2.0       30) libffi/3.4.4-GCCcore-13.2.0     41) GROMACS/2024.1-foss-2023b
  9) libpciaccess/0.17-GCCcore-13.2.0  20) FFTW/3.3.10-GCC-13.2.0           31) Python/3.11.5-GCCcore-13.2.0
 10) hwloc/2.9.2-GCCcore-13.2.0        21) gompi/2023b                      32) gfbf/2023b
 11) OpenSSL/1.1                       22) FFTW.MPI/3.3.10-gompi-2023b      33) cffi/1.15.1-GCCcore-13.2.0

```



### Within a Batch Job

To use the production software stack within a batch job, you need to load the tools/prod module. We advice to purge all modules first and start with a clean slate. 

For example to load GROMACS-2024.1 from the production software stack and use within a batch you, you could do the following:

Using GROMACS-2024.1 from the production software stack in a batch job.

```bash
#PBS -lwalltime=01:00:00
#PBS -lselect=1:ncpus=16:mem=16gb

ml purge
ml tools/prod
ml GROMACS/2024.1-foss-2023b
# optional
ml

mpirun gmx_mpi <your options>
```

This would purge any loaded modules, loads the production software stack, loads the requested program, in this case GROMACS-2024.1 and optional lists all loaded modules. The last `ml` is only to document which other modules are loaded which might be of interest if other software is used at the same time!

## Using the Development Software Stack

### On the Login Nodes

The EasyBuild development software stack may be accessed on the login nodes by first loading the 'tools/dev' module and then the corresponding program module (see the Applications for more advice on using the module system). The following examples demonstrates how to use the development software stack on the login nodes (please remember when loading modules on the login node, it is forbidden to run computational workloads):

Loading the development software stack on the login nodes and viewing available modules.

```console
[username@login ~]$ ml tools/dev
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

Note: the `tools/prod` and `tools/dev` software stack are mutually exclusive, so you can only load one at the time!

Loading the development software stack on the login nodes and loading the GROMACS/2024.1-foss-2023b module.

```console
[username@login ~]$ ml tools/dev
[username@login ~]$ ml GROMACS/2024.1-foss-2023b
[username@login ~]$ ml 
Currently Loaded Modules:
  1) tools/dev                        12) libevent/2.1.12-GCCcore-13.2.0   23) ScaLAPACK/2.2.0-gompi-2023b-fb  34) cryptography/41.0.5-GCCcore-13.2.0
  2) GCCcore/13.2.0                    13) UCX/1.15.0-GCCcore-13.2.0        24) foss/2023b                      35) virtualenv/20.24.6-GCCcore-13.2.0
  3) zlib/1.2.13-GCCcore-13.2.0        14) libfabric/1.19.0-GCCcore-13.2.0  25) bzip2/1.0.8-GCCcore-13.2.0      36) Python-bundle-PyPI/2023.10-GCCcore-13.2.0
  4) binutils/2.40-GCCcore-13.2.0      15) PMIx/4.2.6-GCCcore-13.2.0        26) ncurses/6.4-GCCcore-13.2.0      37) pybind11/2.11.1-GCCcore-13.2.0
  5) GCC/13.2.0                        16) UCC/1.2.0-GCCcore-13.2.0         27) libreadline/8.2-GCCcore-13.2.0  38) SciPy-bundle/2023.11-gfbf-2023b
  6) numactl/2.0.16-GCCcore-13.2.0     17) OpenMPI/4.1.6-GCC-13.2.0         28) Tcl/8.6.13-GCCcore-13.2.0       39) networkx/3.2.1-gfbf-2023b
  7) XZ/5.4.4-GCCcore-13.2.0           18) OpenBLAS/0.3.24-GCC-13.2.0       29) SQLite/3.43.1-GCCcore-13.2.0    40) mpi4py/3.1.5-gompi-2023b
  8) libxml2/2.11.5-GCCcore-13.2.0     19) FlexiBLAS/3.3.1-GCC-13.2.0       30) libffi/3.4.4-GCCcore-13.2.0     41) GROMACS/2024.1-foss-2023b
  9) libpciaccess/0.17-GCCcore-13.2.0  20) FFTW/3.3.10-GCC-13.2.0           31) Python/3.11.5-GCCcore-13.2.0
 10) hwloc/2.9.2-GCCcore-13.2.0        21) gompi/2023b                      32) gfbf/2023b
 11) OpenSSL/1.1                       22) FFTW.MPI/3.3.10-gompi-2023b      33) cffi/1.15.1-GCCcore-13.2.0
```

Note: As evident from the example above, apart from the "gateway" module`tools/dev` only the module for the software you want to use is required to be loaded. Any other module which is required will be loaded now automagically. It does not print out which modules are loaded, per default `ml` *silently* loads the required modules. Also, only the module of the software you want to use is required, all dependencies are automagically resolved. Furthermore, the `-foss-2023b` is what is called a *toolchain*. You only can load software within the same toolchain hierarchy, you cannot mix-and-match. See [here](https://docs.easybuild.io/common-toolchains/#toolchains_diagram) for more information.

### Within a Batch Job

Using the development software stack within a batch job is the same as for using it on the login node. For example to load GROMACS 2021.5 from the development stack and use within a batch you, you could do the following:

Using GROMACS 2021.5 from the development software stack in a batch job.

```bash
#PBS -lwalltime=01:00:00
#PBS -lselect=1:ncpus=16:mem=16gb

ml purge
ml tools/dev
ml GROMACS/2021.5-foss-2021b
# optional
ml

mpirun gmx_mpi <your options>
```

This would purge any loaded modules, loads the production software stack, loads the requested program, in this case GROMACS-2024.1 and optional lists all loaded modules. The last ml is only to document which other modules are loaded which might be of interest if other software is used at the same time!

## Using the GPU Software Stack

As part of the architecture specific installation, we are also building software specifically for our GPU nodes.

### On the login nodes

The production GPU software stack cannot be directly loaded on the login nodes. However, it IS possible to "view" which software has been installed and is therefore available on the compute nodes. This is done the same way as shown in the [Production](#using-the-production-software-stack) software stack section.

Note: the login nodes do not have suitable GPUs to run computational work, thus any attempts to run `CUDA` software will fail.

### Within a batch job

To use the production software stack within a batch job, the `tools/prod` module needs to be used

For example to load GROMACS 2021.3 (with CUDA acceleration) from the production GPU software stack and use within a batch you, you could do the following:

Using production GPU modules within a batch script

```bash
#PBS -lwalltime=01:00:00
#PBS -l select=1:ncpus=4:mem=24gb:ngpus=1:gpu_type=RTX6000

ml purge
ml tools/prod
ml AlphaFold/2.3.4-foss-2022a-CUDA-11.8.0-ColabFold

<your options>
```

More information on running GPU jobs may be found on our [GPU Jobs page](../queues/gpu-jobs.md)

## User Software Development
### Using EasyBuild

In order to make it easier for those users who are either developing software, or need to quickly install some software to test, we are now offering a development module: `tools/eb-dev`.

This allows you to install software with EasyBuild in your own user space, so that you can use a locally modified version of software, or set up a reproducible build environment when working on your own software development projects. The latter will allow you to change your build environment without going through Anaconda or any other virtual environment.
If you only want to switch compilers etc, you can make use of the already installed `buildenv` modules (see below) which will give you convenient access to various versions of both GCC and Intel compilers, next to MPI access.

Note: EasyBuild is quite prescriptive, so if you are selecting a specific tool-chain you will get a specific version of the compiler, MPI etc. It is not a mix-and-match approach.
Note: Remember we got a heterogeneous cluster (CX3-Phase2) regarding the various CPUs we are using (AMD, and Intel). Care needs to be taken when building software from source to make sure it will run on the cluster. Please come to one of our workshops if you are  doing software development seriously.

We will be offering hands-on workshops throughout the year to explain the benefits of using EasyBuild for your software development project for example, or simply how to make the best use of the new and exciting software stack. These workshops will be announced via the usual channels.

#### Example:

As an example, consider the installation of a particular `zlib` version. We are not interested in a particular toolchain as we are only want to use `zlib`. Here the *toolchain* would be called `SYSTEM` as that indicates the tools coming from the Operating System (OS) are being used. 
The first step would be to load the `tools/eb-dev` module:

```bash
$ ml purge
$ ml tools/prod
$ ml tools/eb-dev
```

This will remove any already loaded modules, load the `tools/prod` default module again and then load the `tools/eb-dev` modules. This also will set up the environment for you which can be checked like this:

```shell
$ eb --show-config
```

The output should be something similar like this, with `USERNAME` being your username of course:

```console
#
# Current EasyBuild configuration
# (C: command line argument, D: default value, E: environment variable, F: configuration file)
#
accept-eula-for  (E) = Intel-oneAPI, NVHPC, NVIDIA-driver, CUDA
buildpath        (E) = /dev/shm/USERNAME
containerpath    (E) = /rds/general/user/USERNAME/home/apps/containers
download-timeout (C) = 100.0
installpath      (E) = /rds/general/user/USERNAME/home/apps
optarch          (E) = {'Intel': 'axAVX2 -mavx2', 'GCC': 'march=haswell -mtune=generic'}
packagepath      (E) = /rds/general/user/USERNAME/home/apps/packages
parallel         (E) = 8
prefix           (E) = /rds/general/user/USERNAME/home/apps
repositorypath   (E) = /rds/general/user/USERNAME/home/apps/ebfiles_repo
robot            (C) = /sw-eb/software/EasyBuild/4.9.4/easybuild/easyconfigs
robot-paths      (D) = /sw-eb/software/EasyBuild/4.9.4/easybuild/easyconfigs
sourcepath       (E) = /rds/general/user/USERNAME/home/apps/sources:/rds/easybuild/sources
tmpdir           (E) = /dev/shm/USERNAME
```

bashWithout going into too much details, the software, the modules and any source codes which are downloaded are stored in `~/apps` and this folder will be created automatically the first time you use EasyBuild. The `optarch` is specific to `CX3-Phase2` where we have the already explained mix of AMD and Intel CPUs. If all works out well, both the `Intel` and `GCC` compiler should compile binary packages which should run on both, AMD and Intel CPUs. Usually that is working but sometimes there are hickups. 

Having set up and checked our environment we want to install `zlib`. First step is to search for it using EasyBuild:

```console
$ eb -S zlib
== found valid index for /sw-eb/software/EasyBuild/4.9.4/easybuild/easyconfigs, so using it...
CFGS1=/sw-eb/software/EasyBuild/4.9.4/easybuild/easyconfigs
[ ... ]
 * $CFGS1/z/zlib/zlib-1.3.1-GCCcore-14.2.0.eb
 * $CFGS1/z/zlib/zlib-1.3.1.eb
```

Your list will be much longer as all EasyConfig files which contain the string 'zlib' will be searched and displayed. We are only interested in the last two entries shown here. The `zlib-1.3.1-GCCcore-14.2.0.eb` would install `zlib` which is compiled using the `GCCcore-14.2.0` toolchain. That is not what we want. The `zlib-1.3.1.eb` does not have a toolchain version and thus automatically falls back to `SYSTEM`. 

Installation is simple:

```console
$ eb --trace zlib-1.3.1.eb
```

The `--trace` will give you a bit more information. EasyBuild will first consult the installed module files to see if that particular version of `zlib` is already installed. If it is, it will not build it. If it is not, it will:

- download the source package unless it can be found in the centrally managed source packages folder
- compares the checksum of the downloaded file with the one stored in the EasyConfig file
- unpack the source if required
- installs the software as instructed in the EasyConfig file
- tests the installation
- writes the module file
- finish off the installation by cleaning both the `buildpath` and `tmpdir` locations.

In order to use the so installed software, you only need to load the `tools/eb-dev` module before loading the newly installed software package. 

### Using the buildenv

Sometimes software is not (yet) in EasyBuild, either because the software package is very specific, or it is something which is currently written. Compiling software is usually quite tricky, and big software projects often rely on external libraries and software. Managing this can be a nightmare as quickly you are in what is called *dependency hell*. 
However, there is help around as well. The first thing you need to consider is, which compiler and which compiler version do I want to use. If the software is currently written, we would suggest to go for the latest installed compiler version. This way, you are writing software for a current compiler release and not for something where compilation later will be difficult or impossible as the compiler version might no longer be supported and thus cannot be downloaded. 

#### Example:

In order to see what is the latest installed compilers, simply search for the `buildenv` modules:

```console
$ ml spider buildenv
```

This will return the currently installed compilers, i.e.

```console
------------------------------------------------------------------------------------------
  buildenv:
------------------------------------------------------------------------------------------
    Description:
      This module sets a group of environment variables for compilers, linkers, maths
      libraries, etc., that you can use to easily transition between toolchains when
      building your software. To query the variables being set please use: module show
      <this module name>

     Versions:
        buildenv/default-foss-2022a
        buildenv/default-foss-2022b-CUDA-12.0.0
        buildenv/default-foss-2022b
        buildenv/default-foss-2023a-CUDA-12.1.1
        buildenv/default-foss-2023a
        buildenv/default-foss-2023b
        buildenv/default-intel-2022a
        buildenv/default-intel-2022b
        buildenv/default-intel-2023a
        buildenv/default-intel-2023b

```

Evidently, the latest version for the `foss` compiler is `buildenv/default-foss-2023b`. But what compiler is that actually? The quickest way is simply loading the module and see what else has been loaded:

```console
$ ml buildenv/default-foss-2023b
§ ml
```

These modules are loaded per default:

```console
Currently Loaded Modules:
  1) tools/prod                        14) libfabric/1.19.0-GCCcore-13.2.0
  2) GCCcore/13.2.0                    15) PMIx/4.2.6-GCCcore-13.2.0
  3) zlib/1.2.13-GCCcore-13.2.0        16) UCC/1.2.0-GCCcore-13.2.0
  4) binutils/2.40-GCCcore-13.2.0      17) OpenMPI/4.1.6-GCC-13.2.0
  5) GCC/13.2.0                        18) OpenBLAS/0.3.24-GCC-13.2.0
  6) numactl/2.0.16-GCCcore-13.2.0     19) FlexiBLAS/3.3.1-GCC-13.2.0
  7) XZ/5.4.4-GCCcore-13.2.0           20) FFTW/3.3.10-GCC-13.2.0
  8) libxml2/2.11.5-GCCcore-13.2.0     21) gompi/2023b
  9) libpciaccess/0.17-GCCcore-13.2.0  22) FFTW.MPI/3.3.10-gompi-2023b
 10) hwloc/2.9.2-GCCcore-13.2.0        23) ScaLAPACK/2.2.0-gompi-2023b-fb
 11) OpenSSL/1.1                       24) foss/2023b
 12) libevent/2.1.12-GCCcore-13.2.0    25) buildenv/default-foss-2023b
```

As evident, quite some modules are being loaded and thus the build environment has the following software packages loaded:

- GCC
- OpenSSL
- OpenMPI
- OpenBLAS/FlexiBLAS
- FFTW
- ScaLAPACK

next to some auxiliary programs we don't look too much in here. This would be a very basic build environment. For the sake of this example. we require `cmake`. Thus, we are search for it and load the required module:

```console
$ ml spider cmake
$ ml CMake/3.27.6-GCCcore-13.2.0
```

Note: The same version of `GCCcore` as the already loaded modules is required.

If other libraries or software packages are required, they either can be loaded via the already installed modules, or can be build and installed in the user's home directory as explained above.

