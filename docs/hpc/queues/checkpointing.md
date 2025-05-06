# Checkpointing

!!! info

    This page **has** been rewritten for CX3 Phase 2.

All HPC jobs have a walltime limit so any jobs that exceeds this will be stopped and work may be lost. If a job needs to run longer, progress will need to be saved and a new job submitted to continue the calculation. For some applications this is as simple as saving the current progress to a file and then reloading it later, indeed some applications have built-in methods of doing this. However, if it doesn't it might be possible to use an external tool called [DMTCP](https://github.com/dmtcp/dmtcp) to checkpoint a job.

## Loading DMCTP into your environment
DMCTP is provided by Easybuild so can load it as a module. Please ensure the selected version matches the current toolchain used for the rest of the job (if any). For example, here we are using the GCCcore 11.3.0 tool chain 

```bash
module load tools/prod
module load DMTCP/3.0.0-GCCcore-11.3.0
```

If DMTCP is not available for the toolchain you wish to use, please let us know.
To verify if DMTCP is available in your environment, run the following command:

```console
dmtcp_command --version
```

If DMTCP is properly installed and accessible, it will display the version information.

```console
dmtcp_command (DMTCP) 3.0.0
License LGPLv3+: GNU LGPL version 3 or later
    <http://gnu.org/licenses/lgpl.html>.
This program comes with ABSOLUTELY NO WARRANTY.
This is free software, and you are welcome to redistribute it
under certain conditions; see COPYING file for details.
```

## dmtcp_coordinator
A dmtcp_coordinator process must be started before dmtcp is able to handle checkpoints. There should be exactly one dmtcp_coordinator per job and in the case of multi-node jobs, the coordiantor should be started on the first node. The coordinator will then be able to communicate with all other nodes to run dmtcp_checkpoint and dmtcp_restart which are set via the DMTCP_HOST and DMTCP_PORT environment variables. While the dmtcp_coordinator can be started manually it is generally simpler to use the integrated launcher unless you want to manually control checkpointing.

## Launching a process with checkpointing
The easiest way to enable checkpointing of a given application is to use dmtcp_launch. For example, to enable checkpointing every 10 minutes run the following,
```bash
dmtcp_launch -i 600 ./a.out
```
Here we are setting the checkpoint interval in the launch command but it can also be set via the DMTCP_CHECKPOINT_INTERVAL environmental variable. 
This interval should not be set too aggressively as this will cause significant load on the RDS. Generally we recommend a minimum of 10 minutes but if the checkpoint files are large or your application uses a lot of memory when running this time should be increased. Also note that each checkpoint will be a seperate file which will count against a users RDS quota.

## Restarting from a checkpoint:
When a process is terminated a restart script should be created which is the easiest method to restart.
To restart a program from a previous checkpoint, use the following command:

```bash
./dmtcp_restart_script.sh
```

If you want to restart from an earlier checkpoint or if the restart script wasn't created you can use dmtcp_restart and the matching dmtcp file.
```bash
dmtcp_restart ckpt_a.out_*.dmtcp
```
This method can run into issues and using the dmtcp_restart_script.sh script is generally safer.

## Submitting jobs to PBSPro with DMTCP:
Putting this all together we can build a simple PBSPro script for running a C binrary. You can replace a.out with your application or binrary.

```bash
#!/bin/bash
#PBS -N my_job
#PBS -l select=1:ncpus=3:mem=4gb
#PBS -l walltime=24:00:00

# Load required modules
module load tools/prod
module load DMTCP/3.0.0-GCCcore-11.3.0

# Set DMTCP environment variables
export DMTCP_COORD_PORT=$(shuf -n 1 -i 20000-65535)
export DMTCP_CHECKPOINT_INTERVAL=600
export DMTCP_COORD_HOST="hostname"

cd $PBS_O_WORKDIR

# Start process with DMTCP
dmtcp_launch ./a.out
```

An example of a PBSPro job that restarts an earlier checkpointed process would be,

```bash
#!/bin/bash
#PBS -N my_job
#PBS -l select=1:ncpus=3:mem=4gb
#PBS -l walltime=24:00:00

# Load required modules
module load tools/prod
module load DMTCP/3.0.0-GCCcore-11.3.0

# Set DMTCP environment variables
export DMTCP_COORD_PORT=$(shuf -n 1 -i 20000-65535)
export DMTCP_CHECKPOINT_INTERVAL=600
export DMTCP_COORD_HOST="hostname"

cd $PBS_O_WORKDIR

# Restart process from checkpoint
./dmtcp_restart_script.sh
```

## Important environmental variables
Generally it is fine to use the default settings for DMTCP but there a few settings that can tweeked.

DMTCP_CHECKPOINT_INTERVAL=integer
Time in seconds between automatic checkpoints. Checkpoints can also be initiated manually by typing 'c' into the coordinator. (default: 0, disabled; dmtcp_coordinator only)

DMTCP_HOST=string
Hostname where the cluster-wide coordinator is running. (default: localhost; dmtcp_checkpoint, dmtcp_restart only)

DMTCP_PORT=integer
The port the cluster-wide coordinator listens on. (default: 7779)

DMTCP_GZIP=(1|0)
Set to "0" to disable compression of checkpoint images. (default:0, compression disabled; dmtcp_checkpoint only)

DMTCP_CHECKPOINT_DIR=path
Directory to store checkpoint images in. (default: ./)

DMTCP_SIGCKPT=integer
Signal number to use for checkpointing. Must not be used by the user program. (default: SIGUSR2; dmtcp_checkpoint only)
