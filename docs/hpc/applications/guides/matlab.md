# MATLAB

!!! info

    This page **has** been rewritten for CX3 Phase 2. Not many changes needed

MATLAB is a programming and numeric computing platform used by millions of engineers and scientists to analyse data, develop algorithms, and create models.

## Loading MATLAB

!!! warning 
	We are aware of issues when running array jobs using `MATLAB/2024b`. If you encounter issues with it, please opt for any earlier version.

There are a number of versions installed on the RDS which you can see by first loading into the Production release of our [Easybuild](../easybuild.md) stack and then running `module spider MATLAB`

```console
$ module load tools/prod
$ module spider MATLAB
-------------------------------------------------------------------------------------------------------------------    
  MATLAB:
-------------------------------------------------------------------------------------------------------------------    
    Description:
      MATLAB is a high-level language and interactive environment that enables you to perform computationally
      intensive tasks faster than with traditional programming languages such as C, C++, and Fortran.

     Versions:
        MATLAB/2021a-r8
        MATLAB/2023a_Update_3
        MATLAB/2023b
        MATLAB/2024b
```

You can also run `module spider matlab` (in lowercase) which will show all software that has "matlab" in its name. At the time of writing, this includes only one other package: `CVX/2.2.2-MATLAB-2024b`

## Submitting a Job

To submit a MATLAB job to the PBSPro scheduler you will first need to write a job script. Below is a simple example.

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=16gb
#PBS -l walltime=01:00:00
#PBS -N mdcs_job
 
module load tools/prod
module load MATLAB/2023a_Update_3
 
cd $PBS_O_WORKDIR
 
matlab -nosplash -nodisplay -nojvm -batch myMfile -logfile myTestRun_output.log
```

Going through this line-by-line:

1. This is called the "shebang" and lets PBS know this is a bash script. All jobs submitted to PBS should be bash scripts.
1. PBS directive letting PBS know the compute resources needed to run the MATLAB code. Here the script is asking for 1 node (think 1 computer), with access to 4 CPUS and 16 GB. See New Job sizing guidance for queue limits.
1. PBS directive giving the maximum walltime. This should be the length of time your MATLAB code takes to run. It is advisable to give a bit of margin here but remember the more time your request, the longer your job is likely to be queued.
1. PBS directive giving the job name. This isn't required but can be useful when checking what jobs have run.
1. Load the production instance of our Easybuild software installations.
1. Load the necessary modules.
1. Change directory (`cd`) to the location this script was submitted. This can be any `cd` command but here the `$PBS_O_WORKDIR` environment variable was used as short hand. Have a look at the [General best practices](../../best-practice.md) for information about data locations.
1. Finally the Matlab command to start the MDCS job (`matlab -nosplash -nodisplay -nojvm -batch myMfile -logfile myTestRun_output.log`), with the Matlab script myMfile.m.

Note that this assumes your PBS script and MATLAB script are in the same location on the RDS. To submit the job to PBSPro, use the following command as given in Getting started

```
qsub mdcs.pbs
```

This command will submit the job to PBSPro and return a job ID. You can monitor the status of the job using the following command:

```
qstat
```

Once the job is completed, you can retrieve the results from the output and error files specified in the PBSPro script file (`mdcs_job.o<job_id>` and `mdcs_job.e<job_id>`, respectively).

## MDCS


MATLAB has a number of methods for running in parallel. Below is an example that runs a function in a parfor loop via a local MDCS cluster. By local we mean local to the compute node, so this is unable to run on multiple compute nodes. Currently we are unable to support multi-node MDCS.

### MDCS example

```matlab
% Start a Matlab MDCS job on a PBSPro cluster
% using the parpool function to create a parallel pool
parpool('local', 4);
 
% Define a function to be executed in parallel
my_fun = @(x) x^2;
 
% Create an array of inputs to the function
inputs = 1:10;
 
% Use the parfor loop to execute the function in parallel
outputs = zeros(size(inputs));
parfor i = 1:length(inputs)
    outputs(i) = my_fun(inputs(i));
end
 
% Close the parallel pool
delete(gcp);
```

In this example, we first load the necessary modules using the `module load` command. Then, we start a Matlab MDCS job on the PBSPro cluster by calling the `parpool` function with the argument `'local', 4`, which creates a parallel pool with 4 workers on the local machine.

Next, we define a simple function `my_fun` that takes a single input and returns its square. We also create an array of inputs to this function using the colon operator `(1:10)`.

We then use the `parfor` loop to execute the `my_fun` function on each element of the inputs array in parallel. The `parfor` loop behaves like a regular for loop, except that it splits the iterations across the workers in the parallel pool. Each worker executes a subset of the iterations and returns its results to the client.

Finally, we close the parallel pool by calling the `delete(gcp)` function, which shuts down the workers and releases their resources.

Note that the `parpool` function and the parfor loop are just two examples of the many parallel computing tools available in Matlab. For more information and examples, you can consult the Matlab documentation or visit the [MathWorks website](https://uk.mathworks.com/products/matlab.html).
