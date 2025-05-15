# OpenFOAM

[OpenFOAM](https://www.openfoam.com/) is the free, open source CFD software developed primarily by OpenCFD Ltd since 2004. We have many versions of OpenFOAM available on our HPC systems, and we recommend using the latest version available as shown below.

## Running OpenFOAM on the HPC

You can search which versions of OpenFOAM are available on the HPC by running the following command:

```bash
module load tools/prod
module spider openfoam
```

This will show output similar to the following
```text
-------------------------------------------------------------------------------------------------------------------------------    
  OpenFOAM:
-------------------------------------------------------------------------------------------------------------------------------    
    Description:
      OpenFOAM is a free, open source CFD software package. OpenFOAM has an extensive range of features to solve anything from     
      complex fluid flows involving chemical reactions, turbulence and heat transfer, to solid dynamics and electromagnetics.      

     Versions:
        OpenFOAM/v2206-foss-2022a
        OpenFOAM/v2306-foss-2022b
        OpenFOAM/v2406-foss-2023a
        OpenFOAM/7-foss-2022a-20200508
        OpenFOAM/10-foss-2022a-20230119
```
You can of course also do `module avail openfoam`, both commands are very similar.

To load a specific version of OpenFOAM, you can use the following command:

```bash
module load <version>
#Where version could be any one of the versions listed above, for example
module load OpenFOAM/v2406-foss-2023a
```

## How to get the OpenFOAM commands

Quite often, you will need to use the following command once you loaded the OpenFOAM module. The command below ensures that you will now be able to run all the OpenFOAM commands. Please note the example below is for bash shell and may need to be changed if you are using some other shell.

```bash
source $FOAM_BASH
```

Finally, you can create your job script and submit it to the HPC scheduler. For details on how to create the batch script, please see the [Running Jobs on the HPC](../../getting-started/running-your-first-job.md) guide.
