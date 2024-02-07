# Accessing Software

A large number of software/applications are already installed on our systems and we use the "environment modules" system to enable users to load different applications and different versions of those applications into their environment.

# Environment Modules
The module command is used to access the environment modules system, and load/unload applications from your environment. When you first log into the login node, you will not have any modules load:

```console
$ module list
No Modulefiles Currently Loaded
You can see a list of what applications are installed by running the module avail command (as there are many applications installed, this may take some time):

module avail
------------------------------------------------------------- /apps/modules/4.7.1/modulefiles --------------------------------------------------------------
7zip/9.20                              dolfin/1.7.0                         kgg/2.5                               plink/1.06
7zip/16.02(default)                    dolfin/1.7.0.old                     king/2.0                              plink/1.06-multivariate
7zip/22.01                             dot                                  lammps/1Feb13                         plink/1.07
21cmsimfast                            doxygen/1.8.8(default)               lammps/03Feb10                        plink/1.90
31Mar17                                drmaa/1.0.19                         lammps/4Feb14                         plink/1.90p(default)
abaqus/6.6                             dsdp/5.8(default)                    lammps/5Nov13                         plinkseq/0.10
 
...
```

A program can then be loaded into your environment by running the module load name command:

```console
module load 7zip/22.01
```

Read more about working with installed applications.

# Application Guides
We have Application Guides for some of the most commonly used applications on the HPC facility. For example, users familiar with the Anaconda Data Science platform should review our Conda guide before using it on the system (we also recommend Conda for those uses wishing to manage their own R, Python or Perl environments).

# What to do if an application you need isn't installed
If there's a package you need that isn't installed under modules nor available through conda, please follow the links in our "Get Support" page to raise a software installation request.
