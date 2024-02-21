# Checkpointing

All HPC jobs have a walltime limit and any jobs that exceeds this will be stopped and work can be lost. If a job needs to run longer then any progress will need to be saved and a new job submitted to continue the calculation. For some applications this is as simple as saving the current progress to a file and then reloading it later, indeed some applications have built-in methods of doing this. However, if it does it might be possible to use an external tool called [DMTCP](https://github.com/dmtcp/dmtcp) to checkpoint a job.

## Loading DMCTP into your environment
Before using DMTCP, make sure to load the required software modules. Assuming you have already loaded the `tools/prod` module, load any additional software needed by running the appropriate `module load` commands. For example:

```console
module load tools/prod
module load DMTCP/3.0.0-GCCcore-11.3.0
```

If DMTCP is not available for the toolchain you wish to use, please let us know.

## Check DMTCP Availability
To verify if DMTCP is available in your environment, run the following command:

```console
dmtcp_command --version
```

If DMTCP is properly installed and accessible, it will display the version information.

## Checkpointing a program

DMTCP allows you to create checkpoints of running programs, which can be later restored for continuation. To checkpoint a program, use the following command:

```console
dmtcp_checkpoint <your_program>
```

Replace "<your_program>" with the actual command or script you want to checkpoint. This will create a checkpoint directory containing the necessary files.

## Restarting from a checkpoint:

To restart a program from a previous checkpoint, use the following command:

```console
dmtcp_restart <checkpoint_directory>
```

Replace "<checkpoint_directory>" with the path to the directory containing the checkpoint files.

## Submitting jobs to PBSPro with DMTCP:

If you want to submit a job to PBSPro and utilize DMTCP for checkpointing, you can include DMTCP commands within your PBS script. Here's an example:

```bash
#!/bin/bash
#PBS -N my_job
#PBS -l select=1:ncpus=3:mem=4gb
#PBS -l walltime=24:00:00
 
# Load required modules
module load tools/prod
module load SciPy-bundle/2022.05-foss-2022a
module load DMTCP/3.0.0-GCCcore-11.3.0
 
# Set DMTCP environment variables
export DMTCP_COORD_PORT=7779
export DMTCP_CHECKPOINT_DIR=/path/to/checkpoints
 
# Start DMTCP coordinator
dmtcp_coordinator --daemon --exit-after-ckpt &
 
# Checkpoint your_program
dmtcp_checkpoint python3 your_script.py
```

Make sure to modify the script according to your requirements, such as job name, resource limits, software modules, checkpoint directory path, and the actual command for running your program.

These steps should provide you with a basic understanding of how to use DMTCP in an HPC environment. For more advanced usage and additional options, please refer to the DMTCP documentation or consult with your system administrator.