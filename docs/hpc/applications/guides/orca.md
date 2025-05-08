# ORCA

!!! info

    This page **has** been rewritten for CX3 Phase 2.

## Access to Orca

Access to Orca requires the user to register at the Orca forum, [https://orcaforum.kofo.mpg.de/](https://orcaforum.kofo.mpg.de/). There you will agree to the EULA and receive a confirmation email. Please raise a ticket via [https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/) giving a screen shot of this email and ask to be added to the Orca access list.

## Versions available
There are a few versions of Orca installed on HPC, use the module system to list and then load the desired version

```console
[user@login-ai ~]$ module spider orca

-------------------------------------------------------------------------------------------------------------------------------
  ORCA:
-------------------------------------------------------------------------------------------------------------------------------
    Description:
      ORCA is a flexible, efficient and easy-to-use general purpose tool for quantum chemistry with specific emphasis on
      spectroscopic properties of open-shell molecules. It features a wide variety of standard quantum chemical methods
      ranging from semiempirical methods to DFT to single- and multireference correlated ab initio methods. It can also treat
      environmental and relativistic effects.

     Versions:
        ORCA/5.0.4-gompi-2023a
        ORCA/6.0.0-foss-2023b-avx2
        ORCA/6.0.0-foss-2023b
        ORCA/6.0.0-gompi-2023a-avx2
        ORCA/6.0.1-gompi-2023a-avx2
```
You can of course also do `module avail orca`, both commands are very similar.

## Simple job
To submit a job to the HPC system, you need to create a job submission script that specifies the necessary input files, the ORCA version, and any required input parameters. Here's a simple example you can use to get started:

```bash
#!/bin/bash
#PBS -N orca_job
#PBS -l select=1:ncpus=1:mem=8gb
#PBS -l walltime=01:00:00
#This requests 1 cpu on a single node. 

# Load the ORCA module
module load ORCA/6.0.1-gompi-2023a-avx2

# Change to the job's working directory
cd $PBS_O_WORKDIR

# Run ORCA
orca my_input_file.inp > orca_output.log
```

1. Replace my_input_file.inp with the name of your ORCA input file.
1. In the module load command, replace `ORCA/6.0.1-gompi-2023a-avx2` with the name of the ORCA module as it appears in `module spider` that you want to use.
1. Modify the #PBS directives as needed to specify the resources required by your job, such as the number of nodes, cores, and walltime. Have a look at the [Queueing System](../../queues/index.md) pages for more information.

Save this as orca_job.pbs and then submit the job by running the following command:

```console
qsub orca_job.pbs
```

This will submit the job to PBSPro. The job will run the ORCA executable with the specified input file, and any output will be written to the orca_output.log file in the job's working directory. For more information on using Orca have a look at the official documentation: [https://www.orcasoftware.de/tutorials_orca/](https://www.orcasoftware.de/tutorials_orca/)
