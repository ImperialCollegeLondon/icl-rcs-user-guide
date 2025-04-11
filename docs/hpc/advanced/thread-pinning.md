# Thread Pinning

In this page, we describe some settings and flags that can help you to run your code faster without making any changes in the code or the way you compile your code. Most of the changes that we describe below will need to be added to the job script only.

!!! note

    Please note that the options given below are for Intel compilers and Intel MPI. In some cases, similar options are available for OpenMPI and OpenMP. We do not aim to describe all possible settings and or options. Instead, our aim is to briefly describe what pinning is, how can you do the same and  what advantages it can offer.
    
    Please also note that any advise given below should be first tested on a small example of your code/application (one that mimics your application and finishes quickly) and then only you should use them in your production runs. Based on our experience and knowledge, some applications may benefit extremely from pinning while others may not. So, please test your code.

## What is Thread Pinning?

Thread pinning in general refers to the binding of a process or thread to a specific core. When you run your code on the HPC system, the OS (operating system) kernel does a reasonably good job of mapping processes/threads to physical cores. In some cases, the OS kernel may decide to migrate the thread from one core to another. This migration of thread from one physical core to another brings in some additional overhead and can reduce the speed of your application. We can explicitly specify how our threads should be placed across all available cores and they should not migrate from one core to another. In

Although in some other cases, the OpenMP and MPI runtime libraries can perform certain placements by default, it may not be optimal. It is therefor highly advisable to test a few things to find the optimal settings. Although, with experience, it will become more clear that which of the following can work better.

## Flags to be used for thread pinning

For Intel MPI and intel compilers, the following 5 flags (in general) are all that is necessary to achieve pinning. If your application is not using either of the OpenMP/MPI, you can skip those flags and use the remaining ones. If you are unsure with it, you can still use all 5 of them in your job script and they won't cause any harm. We will not be describing them in detail here. Instead, we will link appropriate web pages so that interested users can refer to those pages.



### Flags for Intel MPI

**I_MPI_DEBUG**:- Print out debugging information when an MPI program starts running. For more information, please see [https://www.intel.com/content/www/us/en/docs/mpi-library/developer-reference-linux/2021-8/other-environment-variables.html](https://www.intel.com/content/www/us/en/docs/mpi-library/developer-reference-linux/2021-8/other-environment-variables.html).

**I_MPI_PIN_DOMAIN**:- This environment variable is used to define a number of non-overlapping subsets (domains) of logical processors on a node, and a set of rules on how MPI processes are bound to these domains. For more information, please see [https://www.intel.com/content/www/us/en/docs/mpi-library/developer-reference-windows/2021-8/interoperability-with-openmp-api.html](https://www.intel.com/content/www/us/en/docs/mpi-library/developer-reference-windows/2021-8/interoperability-with-openmp-api.html).

**I_MPI_PIN_ORDER**:- This environment variable defines the mapping order for MPI processes to domains as specified by the I_MPI_PIN_DOMAIN environment variable ([https://www.intel.com/content/www/us/en/docs/mpi-library/developer-reference-windows/2021-8/interoperability-with-openmp-api.html#GUID-525A9B9B-E2D8-4182-AA9F-AEBCE6D31B63](https://www.intel.com/content/www/us/en/docs/mpi-library/developer-reference-windows/2021-8/interoperability-with-openmp-api.html#GUID-525A9B9B-E2D8-4182-AA9F-AEBCE6D31B63)).


### Flags for OpenMP

**OMP_NUM_THREADS**:- This environment variable defines the number of threads that should be used to execute a parallel region. This flag is common for both the Intel compiler and Gnu compilers ([https://www.openmp.org/spec-html/5.0/openmpse50.html](https://www.openmp.org/spec-html/5.0/openmpse50.html)).

**KMP_AFFINITY**:- This environment variable binds the OpenMP threads to physical processing units. For more details, please see [https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/developer-guide-reference/2023-0/thread-affinity-interface.html](https://www.intel.com/content/www/us/en/docs/dpcpp-cpp-compiler/developer-guide-reference/2023-0/thread-affinity-interface.html).

As we mentioned above, similar settings exist for non Intel compilers and we advise the users to consult this page to replace the appropriate setting [https://hpc-wiki.info/hpc/Binding/Pinning](https://hpc-wiki.info/hpc/Binding/Pinning).


### Typical job script using above options on our cluster

Let us see a typical job script that uses above options and then we will describe a few settings that a user can use.

```bash
#PBS -l walltime=00:05:00
#PBS -l select=1:ncpus=64:mem=200gb:mpiprocs=16:ompthreads=4
#PBS -N pinning
#PBS -o output.txt
#PBS -e error.txt
 
# Load the modules
module load imkl-FFTW/2023.1.0-iimpi-2023a
 
#***********************************************************
# Pinning option goes here
export I_MPI_DEBUG=5
export I_MPI_PIN_DOMAIN=omp
export I_MPI_PIN_ORDER=bunch
 
export OMP_NUM_THREADS=4
export KMP_AFFINITY=verbose, scatter
#***********************************************************
 
cd $PBS_O_WORKDIR
 
 
# Your job specific command goes here.
lammps_path="/gpfs/home/lragta/RCS_help/1_janet/lkr/src/lmp_intel_cpu_intelmpi"
 
temp1=900
ex=0.0
ey=0.0
ez=0.0
 
mpirun -v6 -np 16 ${lammps_path} -l log.lammps -nocite -var temp1 ${temp1} -var ex ${ex} -var ey ${ey} -var ez ${ez} -in alkyl-amor-nvt.in
```

### Examples and explanation of some of the pinning options
To make the explanation easy and realistic, we will show the usage of above options on the [HX1 cluster](../hx1.md). For understanding the content below, you only need to note that each of the node on HX1 cluster contains 2 Intel processors. Each processor has 32 cores (a total of 64 cores on each node). There are 2 sockets (hardware in which we put in the actual chip) on each node, one for each processor.

The core rank 0-31 are on first processor while the core numbers 32-63 are on second processor. Hyperthreading (HT) is turned Off on all nodes.

For all examples given below, we used all cores on one node (i.e. total of 64 cores).

#### Setting 1

Let us suppose that we want to make sure that each MPI process spawns 4 threads (`export OMP_NUM_THREADS=4`) and that the different MPI processes should be placed as close as possible (`export I_MPI_PIN_ORDER=bunch`).

For each MPI process, all openMP threads should be placed close to each other (`export KMP_AFFINITY=compact`). We can use the following in our script
 
```bash
export I_MPI_DEBUG=5
export I_MPI_PIN_DOMAIN=omp
export I_MPI_PIN_ORDER=bunch
export OMP_NUM_THREADS=4
export KMP_AFFINITY=verbose,compact
```

The option export `I_MPI_DEBUG=5` will print useful information about MPI process pinning in the output file while the option `export KMP_AFFINITY=verbose` will do the same for OpenMP.

The option export `I_MPI_PIN_DOMAIN=omp` tell that the logical domain size of an MPI process is equal to the number of threads that it can spawn and is controlled by `OMP_NUM_THREADS`. In any job, the product of "mpiprocs" and "ompthreads" should be equal to the number of cores on a node i.e.

> mpiprocs * ompthreads = number of cores on node

The number of MPI processes that you need to specify will depend on the number of nodes requested and is given by the formula

> Total_Number_of_mpi_processes = mpiprocs * number of nodes requested

Based on the above calculations, we can clearly see that we can only launch 16 mpi processes and this is what we used in our mpirun command.

##### Output 1
The options under setting 1 will lead to the following placement of processes and cores.

```console
[0] MPI startup(): 0       2251756  hx1-c11-4-3  {0,1,2,3}
[0] MPI startup(): 1       2251757  hx1-c11-4-3  {4,5,6,7}
[0] MPI startup(): 2       2251758  hx1-c11-4-3  {8,9,10,11}
[0] MPI startup(): 3       2251759  hx1-c11-4-3  {12,13,14,15}
:
[0] MPI startup(): 14      2251770  hx1-c11-4-3  {56,57,58,59}
[0] MPI startup(): 15      2251771  hx1-c11-4-3  {60,61,62,63}
```

#### Setting 2

Now let us suppose that we want to make sure that MPI processes are placed as far as possible in the round-robin fasion i.e. first MPI rank should be placed on core 0 (with ability to spawn 4 threads), 2nd MPI rank on first core of second processor (i.e. core 31) and so on. We only need to make a small change in the job script as shown below.

```bash
export I_MPI_DEBUG=5
export I_MPI_PIN_DOMAIN=omp
export I_MPI_PIN_ORDER=scatter
export OMP_NUM_THREADS=4
export KMP_AFFINITY=verbose,compact
```
##### Output 2

The options under settings 2 will lead to

```console
[0] MPI startup(): Rank    Pid      Node name    Pin cpu
[0] MPI startup(): 0       2252747  hx1-c11-4-3  {0,1,2,3}
[0] MPI startup(): 1       2252748  hx1-c11-4-3  {32,33,34,35}
[0] MPI startup(): 2       2252749  hx1-c11-4-3  {4,5,6,7}
[0] MPI startup(): 3       2252750  hx1-c11-4-3  {36,37,38,39}
:
[0] MPI startup(): 14      2252761  hx1-c11-4-3  {28,29,30,31}
[0] MPI startup(): 15      2252762  hx1-c11-4-3  {60,61,62,63}
```

#### Setting 3

If you are more curious on what happens if you use the scatter option for KMP_affinity while keeping everything same as mentioned in Setting 2, it turns out that the placement remains the same as shown in output 2 (not repeated again).

```console
export I_MPI_DEBUG=5
export I_MPI_PIN_DOMAIN=omp
export I_MPI_PIN_ORDER=scatter
export OMP_NUM_THREADS=4
export KMP_AFFINITY=verbose,scatter
```

#### Setting 4

Now let us assume that you want to use one MPI rank per processor. Each MPI rank can spawn as many threads as all available cores on the processor. You can use something like 

```console
export I_MPI_DEBUG=5
export I_MPI_PIN_DOMAIN=socket
#export I_MPI_PIN_ORDER=scatter # this option is not required for this setting.
export OMP_NUM_THREADS=32
export KMP_AFFINITY=verbose,compact
```

##### Output 4

The options under settings 4 will lead to

```console
[0] MPI startup(): 0       2254327  hx1-c11-5-4  {0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31}
[0] MPI startup(): 1       2254328  hx1-c11-5-4  {32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63}
```

#### Setting 5

Now let us assume that you are running a pure MPI application and you would like to ensure that MPI processes are placed close to each other. You can use something like

```bash
export I_MPI_DEBUG=5
export I_MPI_PIN_DOMAIN=core
#export I_MPI_PIN_ORDER=scatter #The default value of compact will be used.
export OMP_NUM_THREADS=1
export KMP_AFFINITY=verbose,scatter
```

##### Output 5

The options under settings 5 will lead to the following (because of the default value of the compact).

```console
[0] MPI startup(): Rank    Pid      Node name    Pin cpu
[0] MPI startup(): 0       1121779  hx1-c03-8-3  {0}
[0] MPI startup(): 1       1121780  hx1-c03-8-3  {1}
[0] MPI startup(): 2       1121781  hx1-c03-8-3  {2}
[0] MPI startup(): 3       1121782  hx1-c03-8-3  {3}
:
[0] MPI startup(): 62      1121841  hx1-c03-8-3  {62}
[0] MPI startup(): 63      1121842  hx1-c03-8-3  {63}
```
#### Setting 6

Now let us assume that you are running a pure MPI application and you would like to ensure that MPI processes are placed as far as possible to each other. You can use something like

```bash
export I_MPI_DEBUG=5
export I_MPI_PIN_DOMAIN=core
export I_MPI_PIN_ORDER=scatter
export OMP_NUM_THREADS=1
export KMP_AFFINITY=verbose,scatter
```

##### Output 6

The options under settings 6 will lead to the following (because of the default value of the compact).

```console
[0] MPI startup(): Rank    Pid      Node name    Pin cpu
[0] MPI startup(): 0       2556368  hx1-c03-2-4  {0}
[0] MPI startup(): 1       2556369  hx1-c03-2-4  {32}
[0] MPI startup(): 2       2556370  hx1-c03-2-4  {1}
[0] MPI startup(): 3       2556371  hx1-c03-2-4  {33}
:
[0] MPI startup(): 62      2556430  hx1-c03-2-4  {31}
[0] MPI startup(): 63      2556431  hx1-c03-2-4  {63}
```

This concludes our brief introduction of various pinning options that you may use in your application. We again ask you to test them before using any of them as it is. You may be surprised both by the positive speedup or by the reverse.