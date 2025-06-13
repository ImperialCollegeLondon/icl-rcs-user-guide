# R

We have many versions of R already available via [EasyBuild](../easybuild.md) ready for you to use that are optimised for our systems.

## EasyBuild R

To view the versions of R available for you to use, you can do:

```console
[user@login ~]$ module purge
[user@login ~]$ module load tools/prod
[user@login ~]$ module spider R
```

`module purge` is useful if you have loaded other modules that may interfere, otherwise it's not necessary. This will also load the gateway module `tools/prod` which will allow you to view modules which are currently in the production software stack.

To load `R/4.2.1-foss-2022a` as an example, you can do the following:

```console
[user@login ~]$ module load tools/prod
[user@login ~]$ module load R/4.2.1-foss-2022a
```

Remember, that the login nodes are not for running extensive calculations, especially if they are heavy on the file system. If you need to run jobs interactively, please look at the [interactive queue section](../../queues/job-sizing-guidance.md#interactive).

## Conda and R
To begin, following the instructions in the [Conda application guide](./conda.md) to setup Conda for your account. Assuming you have done this, the following set of instructions would create an environment called `r413` containing R version 4.1.3.

```console
[user@login ~]$ eval "$(~/miniforge3/bin/conda shell.bash hook)"
[user@login ~]$ conda create -n r413 r-base=4.1.3 -c conda-forge
[user@login ~]$ source activate r413
```

You should then see `(r413)` on the left hand side which indicates you are in that environment. For installing R packages, it is generally best to stick to the conda-forge channel or the R channel. For example, to install additional packages from the R channel:

```console
[user@login ~]$ conda search -c r "r-*" 
[user@login ~]$ conda install -c r r-png 
```

An activated environment can be deactivated using:

```console
[user@login ~]$ conda deactivate
```

### Bioconductor

Follow the instructions above to create a suitable conda environment, such as `r413`. Once you have created this environment, you can search for bioconductor packages by running the following:

```console
[user@login ~]$ conda search bioconductor-*
```

and you can install bioconductor packages such as bioconductor-teqc by running:

```console
[user@login ~]$ conda install bioconductor-teqc
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

!!! info 
	In order for this to work correctly, you must make sure to request the `ompthreads` flag in your jobscript. For example: `#PBS -lselect=1:ncpus=8:ompthreads=8:mem=4gb`	

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

