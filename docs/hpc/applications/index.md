# Applications

!!! info

    This page **has** been rewritten for CX3 Phase 2.

HPC systems include lots of different compilers, tools, and applications so it’s easy for command names and file paths to clash. We also need to support multiple versions of the same software. To make this easier for users, we use a "module" system that lets you find what programs and their versions are installed on the system, and load and switch between these.

When you start a session or job, only the basic system commands are available by default. If you need access to specific compilers, tools, or applications, you can use the module command to load them. This temporarily updates your environment, making everything in the selected package available for the duration of your session or job. You can even load multiple modules at once if needed.

The best feature of modules is that when you switch to different versions of applications, you need only to change the module command in your script, all the other commands remain unchanged. This makes switching to different versions very easy and you can do it on a per job basis.

## Finding what applications are installed

Most of the software on Imperial's HPC clusters is installed using the [EasyBuild](./easybuild.md) software installation system. When you first log in, you will have the `tools/prod` module loaded; this module is used to make our "production" applications accessible to your module commands. You can see this with the `module list` command:

```bash
[user@login ~]$

Currently Loaded Modules:
  1) tools/prod
```

You can use the `module avail` command to see what compilers, applications and tools are available for you to use. For example:

```bash
[user@login ~]$ module avail
----------------------------------------- /sw-eb/modules/all ------------------------------------
   ABINIT/9.6.2-foss-2022a
   ABINIT/9.6.2-intel-2022a
   ABINIT/9.10.3-intel-2022a
   ABINIT/10.2.5-intel-2023a                         (D)
   AFNI/24.0.02-foss-2023a
   ANTs/2.3.5-foss-2021a
   ATK/2.36.0-GCCcore-10.3.0
   ATK/2.38.0-GCCcore-11.3.0
   ATK/2.38.0-GCCcore-12.2.0
   ATK/2.38.0-GCCcore-12.3.0
   ATK/2.38.0-GCCcore-13.2.0                         (D)
   AUGUSTUS/3.5.0-foss-2023a
   Abseil/20230125.2-GCCcore-12.2.0
   Abseil/20230125.3-GCCcore-12.3.0
   Abseil/20240116.1-GCCcore-13.2.0
   Abseil/20240722.0-GCCcore-13.3.0                  (D)
   Advisor/2023.2.0
   AlphaFold/2.3.1-foss-2022a-CUDA-11.8.0
   AlphaPulldown/2.0.0b4-foss-2022a-CUDA-11.8.0
   Archive-Zip/1.68-GCCcore-13.2.0
   Archive-Zip/1.68-GCCcore-13.3.0                   (D)
   Armadillo/11.4.3-foss-2022b
   Armadillo/12.6.2-foss-2023a
   Armadillo/12.8.0-foss-2023b
   Armadillo/14.0.3-foss-2024a                       (D)
   Arrow/6.0.0-foss-2021b
--More--
```

You can view the versions of a single application by running either `module avail <name>`; note that the `module avail` command is **case-insensitive**. For example:

```bash
[user@login ~]$ module avail GROMACS

----------------------------------------- /sw-eb/modules/all ------------------------------------
   GROMACS/2021.3-foss-2021a-CUDA-11.3.1     GROMACS/2023.1-foss-2022a-CUDA-11.7.0
   GROMACS/2021.3-foss-2021a-PLUMED-2.7.2    GROMACS/2023.1-foss-2022a
   GROMACS/2021.3-foss-2021a                 GROMACS/2023.3-foss-2023a
   GROMACS/2021.5-foss-2021b-PLUMED-2.8.0    GROMACS/2024.1-foss-2023b             (D)
   GROMACS/2021.5-foss-2021b

  Where:
   D:  Default Module

```

## Loading modules

Modules are loaded into your environment using the `module load` command; note that the `module load` command is **case-sensitive**.

If you load a module for an application into your environment without specifying the version you will load the default version (marked with a `(D)` as above). The default version of a module will normally be the latest version we have installed unless we have set a particular version to be the default. We generally recommend that you use the latest installed version of an module unless there are specific reasons not to do so. For example:

```bash
[user@login ~]$ module load GROMACS
[user@login ~]$ module list

Currently Loaded Modules:
  1) tools/prod                        22) FFTW.MPI/3.3.10-gompi-2023b
  2) GCCcore/13.2.0                    23) ScaLAPACK/2.2.0-gompi-2023b-fb
  3) zlib/1.2.13-GCCcore-13.2.0        24) foss/2023b
  4) binutils/2.40-GCCcore-13.2.0      25) bzip2/1.0.8-GCCcore-13.2.0
  5) GCC/13.2.0                        26) ncurses/6.4-GCCcore-13.2.0
  6) numactl/2.0.16-GCCcore-13.2.0     27) libreadline/8.2-GCCcore-13.2.0
  7) XZ/5.4.4-GCCcore-13.2.0           28) Tcl/8.6.13-GCCcore-13.2.0
  8) libxml2/2.11.5-GCCcore-13.2.0     29) SQLite/3.43.1-GCCcore-13.2.0
  9) libpciaccess/0.17-GCCcore-13.2.0  30) libffi/3.4.4-GCCcore-13.2.0
 10) hwloc/2.9.2-GCCcore-13.2.0        31) Python/3.11.5-GCCcore-13.2.0
 11) OpenSSL/1.1                       32) gfbf/2023b
 12) libevent/2.1.12-GCCcore-13.2.0    33) cffi/1.15.1-GCCcore-13.2.0
 13) UCX/1.15.0-GCCcore-13.2.0         34) cryptography/41.0.5-GCCcore-13.2.0
 14) libfabric/1.19.0-GCCcore-13.2.0   35) virtualenv/20.24.6-GCCcore-13.2.0
 15) PMIx/4.2.6-GCCcore-13.2.0         36) Python-bundle-PyPI/2023.10-GCCcore-13.2.0
 16) UCC/1.2.0-GCCcore-13.2.0          37) pybind11/2.11.1-GCCcore-13.2.0
 17) OpenMPI/4.1.6-GCC-13.2.0          38) SciPy-bundle/2023.11-gfbf-2023b
 18) OpenBLAS/0.3.24-GCC-13.2.0        39) networkx/3.2.1-gfbf-2023b
 19) FlexiBLAS/3.3.1-GCC-13.2.0        40) mpi4py/3.1.5-gompi-2023b
 20) FFTW/3.3.10-GCC-13.2.0            41) GROMACS/2024.1-foss-2023b
 21) gompi/2023b
```

When you load a single module on our HPC system, you will often find many other modules are automatically loaded. These modules will be "dependencies" of the module that you loaded and will have been automatically defined by [EasyBuild software](./easybuild.md).

Modules are required at compile and run time so must be present in your qsub file when you submit a job.

## Loading a specific version of an application

In the above example, the default version of the GROMACS application at the time the command was run was `2024.1-foss-2023b` and this is what was loaded.

A specific version of an application can be loaded by specifying the application name and version when running the module load command:

```bash
[user@login ~]$ module load GROMACS/2023.1-foss-2022a-CUDA-11.7.0
```

## Unloading and purging modules

You can purge all the loaded modules from your environment by running the `module purge` command:

```bash
[user@login ~]$ module purge
[user@login ~]$ module list
No modules loaded
```

You may wish to do this if you've accidentally loaded the wrong modules. To load the production modules into your environment again, you can use the `module load` command:

```bash
[user@login ~]$ module load tools/prod
[user@login ~]$ module list

Currently Loaded Modules:
  1) tools/prod
```

## Summary of module commands

| Module Command | Description |
| -------------- | ----------- |
| `module list` | Lists all modules that are currently loaded in your environment. |
| `module load <name>` | Loads the module called <name\> (case-sensitive). |
| `module avail` | Report the names and versions of all the applications that can be loaded. |
| `module avail <name>` | Reports all the versions of an application that matches the name <name\> (case-insensitive). |
| `module help <name>` | Gives information about the module called <name\>, similar to man <name\>. |
| `module purge` | Unloads any loaded modules. We strongly recommend you place this in your submission script before loading any new modules. |
| `module show <name>` | Displays will list the full path of the modulefile and the environment changes the modulefile will make if loaded. |
