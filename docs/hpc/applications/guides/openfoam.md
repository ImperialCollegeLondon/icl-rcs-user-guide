# OpenFOAM

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

[OpenFOAM](https://www.openfoam.com/) is the free, open source CFD software developed primarily by OpenCFD Ltd since 2004. We have many versions of OpenFOAM available on our HPC systems, and we recommend using the latest version available as shown below.

## Running OpenFOAM on the HPC

You can search which versions of OpenFOAM are available on the HPC by running the following command:

```bash
module load tools/prod
module avail -i openfoam
```

This will show output similar to the following
```bash
OpenFOAM/8-foss-2020b            OpenFOAM/v2006-foss-2020a  
OpenFOAM/8-foss-2022a            OpenFOAM/v2106-foss-2021a  
OpenFOAM/10-foss-2022a           OpenFOAM/v2206-foss-2022a  
OpenFOAM/10-foss-2022a-20230119
```

We highly recommend to use the software under `sw-eb/modules/all` whenever you can. These are our production grade software which are built and optimized for our HPC systems.

To load a specific version of OpenFOAM, you can use the following command:

```bash
module load <version>
#where version could be any one of the versions listed above
```

## How to get the OpenFOAM commands

Quite often, you will need to use the following command once you loaded the OpenFOAM module. The command below ensures that you will now be able to run all the OpenFOAM commands. Please note the example below is for bash shell and may need to be changed if you are using some other shell.

```bash
source $FOAM_BASH
```

Finally, you can create your job script and submit it to the HPC scheduler. For details on how to create the batch script, please see the [Running Jobs on the HPC](../../getting-started/running-your-first-job.md) guide.
