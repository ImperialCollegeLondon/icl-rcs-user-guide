# Accessing Software

!!! info

    This page has been partially rewritten for CX3 Phase 2.

A large number of software/applications are already installed on our systems and we use a "[modules](#modules)" system to enable users to load different applications and different versions of those applications into their environment.

# Modules

The `module` command is used to access the modules system, and load/unload applications from your environment. When you first log into the system, you will only have the `tools/prod` module load which provides you access to our "production" modules.

```console
[user@login ~]$ module list

Currently Loaded Modules:
  1) tools/prod
```

You can see a list of what applications are installed by running the module avail command (as there are many applications installed, this may take some time):

```console
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


A program can then be loaded into your environment by running the module load name command:

```console
[user@login ~]$ module load GROMACS
```

Please see the [Loading Applications](../applications/index.md) page for more advice on using the module system.

# Application Guides

We have [Application Guides](../applications/guides/index.md) for some of the most commonly used applications on the HPC facility. For example, those users who are using Python should review our [Python guide](../applications/guides/python.md) before using it on the system.

# What to do if an application you need isn't installed

If there is an application you need that isn't currently [installed on the system](../applications/index.md) and you are unable to build it yourself, then you may need to raise a software installation request with us.

Before raising such a request (which you can do so from our [Getting Support](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/) page), please be mindful that if the application is old, incompatible with modern supported operating systems or particularly complex to install, we may not be able to install it!