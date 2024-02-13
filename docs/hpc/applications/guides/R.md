# R

As of January 2023, we recommend you use the version of R available within the new high performant software which is build locally on the compute nodes using the [easybuild](../easybuild.md) software build and installation framework. The big advantage here is next to the ~1100 already installed R-modules, the software has been build against the used CPU architecture and thus should run with the maximum efficiency possible.

## EasyBuild R

There is no need to use any other modules for R other than the ones being mentioned here. Currently we have two versions of R installed, which can be viewed like this:

```console
module purge
module add tools/prod
module av R
```

This will load the gateway `module tools/prod` which will allow you to view modules which are currently in the production software stack, next to the older, to be deprecated R modules. 

For the submission script this needs to be used:

```console
module purge
module add tools/prod
module add R/4.2.1-foss-2022a
```

This will purge any existing modules to make sure there is no interference, load the gateway module `tools/prod` and loads the currently latest version of R (R/4.2.1-foss-2022a).

In order to use R on the login node, say for a quick testing, you can replace the tools/prod module with the `tools/dev`, for development, and load the required R module.

Remember, that the login nodes are only there for pre- and post-processing of jobs, not for running extensive calculations, specially if they are heavy on the file system. If you need to run jobs interactively, please refer to the interactive job-requests to the queue.

## Anaconda R
To begin, following the instructions in the [Conda application guide](./conda.md) to setup Conda for your account. Assuming you have done this, the following set of instructions would create an environment called r413 containing R version 4.1.3.

```console
module load anaconda3/personal
conda create -n r413 r-base=4.1.3 -c conda-forge
source activate r413
```

You should then see "(r413)" on the left hand side which indicates you are in that environment. For installing R packages it is generally best to stick to the conda-forge channel or the R channel. For example, to install additional packages from the R channel

```console
conda search -c r "r-*" 
conda install -c r r-png 
```

An activated environment can be deactivated using:

```console
conda deactivate
```

### Bioconductor

Follow the instructions above to create a suitable conda environment, such as r413. Once you have created this environment, you can search for bioconductor packages by running the following:

```console
conda search bioconductor-*
```

and you can install bioconductor packages such as bioconductor-teqc by running:

```console
conda install bioconductor-teqc
```

## RStudio

Please note that access to RStudio is provided by the [Open OnDemand](./openondemand.md) service.

## Running R in Parallel

There are a few ways to run R in Parallel. Probably the simplest is to use [Array Jobs](../../queues/array-jobs.md) to split your work into separate subjobs and pull results together at the end. If this isn't possible then the next simplest is to use the [doParallel](https://cran.r-project.org/web/packages/doParallel/index.html) library within R. There are a few ways to use this library so examples have been given for each. To test these methods a mostly non-sense function is created that will take the place of your work function. Ideally this function should take some seconds or more to run, otherwise the overhead of creating/destroying parallel processes is unlikely to be worth it.

```R
fun <- function(n) {
    size = 1000
    start = size*n
    end = size*(n+1)
    startData = start:end
    endData <- 0:size;
    foreach (i=0:size) %do% {
        endData[i] <- startData[i]^2
    }
    return (sum(endData))
}
```

This test function manually creates a vector, takes the square of each element, and then returns the sum. Again this function isn't optimised and it just a place holder. Users should take the time to optimise their equivalent function(s) as much as possible.

### Getting the right number of cores

You can detect the correct number of cores available to your job using `future::availableCores()`. The function within the `parallel::detectCores()`, will report the wrong number, the total number of cores in the node, which are not available to you. Using this one when parallelising your code might have unexpected consequences. In other words, you can use the following code snippet to get the maximum number of cores available to you:

```R
library("future")
numOfCores = future::availableCores()
```

### Foreach+dopar

#### Creating a cluster

It is generally better to create and destroy the cluster to ensure the number of CPUs is set correctly. `numOfCores` should match the total number of CPUs requested in the PBS script, as described above. 

```R
library(doParallel)
numOfCores=4
cl <- makeCluster(numOfCores, type="FORK")
registerDoParallel(cl)
parallelResults <- c()
```

#### Updating Loop

If you already have the function being called in `foreach` loop then it can be very easy to substitute the `%do%` for `%dopar%`

```R
dataRange <- 1:100;
parallelResults <- c();
foreach (n=dataRange) %dopar%  {
    parallelResults[n] <- fun(n)
}
```

### mclapply
This is the parallel version of lapply and often one can be replaced with the other. The advantage here is the you don't have to create/destroy the cluster, the disadvantage is everything must fit inside of lapply. Again make sure `numOfCores` matches the number of cpus you are requesting in PBS.

```R
library(doParallel)
dataRange <- 1:100;
numOfCores=4
ParallelResults <- mclapply(dataRange, fun,mc.cores=numOfCores)
```

### Comparing all Methods

We can see that `mclapply` is the fastest although again the flexibility of `foreach` might make that the preferred method for a lot of workflows.

```R
The estimated time using foreach: 14.834 seconds
The estimated time using foreach+dopar: 3.988 seconds
The estimated time using lapply() function: 14.276 seconds
The estimated time using mclapply() function: 3.722 seconds
```

With `numOfCores=4`

