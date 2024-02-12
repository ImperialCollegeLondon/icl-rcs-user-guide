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