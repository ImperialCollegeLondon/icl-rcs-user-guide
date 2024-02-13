# Profiling an Application

In most simple terms, application profiling refers to the task of running your application and identifying areas which are critical for improvement by using some performance metrics ([https://hpc-wiki.info/hpc/Performance_profiling](https://hpc-wiki.info/hpc/Performance_profiling)). While there are many tools available in the market to profile your applications, they differ in the ease of operations and how much details they can identify for your applications. Some profilers can be just command line based while some others offer a combination of both the GUI and command line.

Some of the famous profilers are:-

* Intel Vtune ([https://www.intel.com/content/www/us/en/developer/tools/oneapi/vtune-profiler.html](https://www.intel.com/content/www/us/en/developer/tools/oneapi/vtune-profiler.html))
* Arm MAP ([https://developer.arm.com/documentation/102732/latest/](https://developer.arm.com/documentation/102732/latest/))
* Total View ([https://help.totalview.io/](https://help.totalview.io/))
* Tau ([http://www.cs.uoregon.edu/research/tau/home.php](http://www.cs.uoregon.edu/research/tau/home.php))
* HPCToolkit ([http://hpctoolkit.org/](http://hpctoolkit.org/))

For this tutorial, we will be focusing on the Intel Vtune profilers. This is because we have Vtune available on the HX1 cluster and we plan to install it on all of our compute clusters. If you find some tool is not there, you should be able to install them on your own in your home directory and use it from there. Now a days, the Intel profilers are free and can be downloaded as a part of the OneAPI toolkit or standalone software. We will not be sharing details on how to install the software (if not already available). You should be able to figure that out by yourself after going through the tools webpage.

We will be sharing some scripts and commands so that you can use these tools on our clusters and profile your applications.

## Intel Vtune
Intel Vtune consists of a range of tools that can serve various purposes. We will  be focusing on the following three:-

* [Intel Advisor](./intel-advisor.md)
* [Intel Application Performance Snapshot (APS)](./intel-aps.md)
* [Intel Vtune Profiler](./intel-vtune-profiler.md)

!!! Note

    For all tools, we are assuming that you will be using Intel compilers to compile your applications and Intel MPI for mpi based applications. For applications which are based on non Intel compilers (and/or non Intel MPI), the tools will still work. However, the commands and overall process may change slightly. You would need to consult the documentation to figure out the exact changes that may be required.