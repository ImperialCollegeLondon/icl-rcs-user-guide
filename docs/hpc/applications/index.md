# Applications

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

As the HPC systems support a large number of different compilers, applications and tools there is a potential problem of conflicting command and path names. In addition we want to be able to offer multiple versions of the same program. We use the module system to provide this in a simple and flexible way.

With the module system, only the usual system commands are available to you at the start of your session or job. If you wish to use a particular compilers, applications or tools, you can make them available to your job or session by using the module command. This will temporarily update your environment to make all the elements of the chosen package available to you for the duration of the job or session, you can have multiple modules loaded concurrently.

The best feature of modules is that when you switch to different versions of applications, you need only to change the module command in your script, all the other commands remain unchanged. This makes switching to different versions very easy and you can do it on a per job basis.

You can use the `module avail` command to see what compilers, applications and tools are available for you to use. Details of modules with special instructions are given on our applications pages. Note that packages with restricted access will not be listed unless you are authorised to use them. Even the compilers are installed using the module system, so we can make different versions available to you as necessary. Modules are required at compile and run time so must be present in your qsub file when you submit a job.

Normally you should use the default version of a module as this will usually be our recommended version. Often newer versions of packages become available and we are happy to install them. In this case you will need to specify the module and the version to get the newer one. For example, instead of module load application you might need to use module load/application/3.2 where version 3.2 is the newer version which you want to use.

The default versions of applications are changed infrequently to avoid disruption to users. Usually this will be done to coincide with new versions of the compilers and libraries and we will announce the change in advance.

| Module Command | Description |
| -------------- | ----------- |
| `module load <name>` | Loads the module called <name\>. |
| `module avail` | Lists all available modules for applications, compilers and tools that can be found on the system. |
| `module list` | Lists all modules that are currently loaded in your environment. |
| `module help <name>` | Gives information about the module called <name\>, similar to man <name\>. |
| `module purge` | Unloads any loaded modules. We strongly recommend you place this in your submission script before loading any new modules. |
| `module show <name>` | Displays will list the full path of the modulefile and the environment changes the modulefile will make if loaded. |
