# How it works

You will be used to logging into a Linux or Unix system, typing in commands to the shell, running programs and viewing data. Once you are finished, you log out and your session ends. The queuing system reproduces this environment, and runs the same shell and commands as interactive login sessions. The difference is that the script is run under the control of the system at a time which it chooses, and thus a connection to a terminal window from the session is not used. The standard input (stdin) for the session is read from the script file which you provide and the standard output and error files (stdout and stderr) are returned as file(s) in your directory when the job is completed.

You initiate the job using the qsub command. This takes your script and passes control to PBSPro to have it executed. You can add parameters to the qsub command to specify the resources the job will need and to flag various options to the system. You can also put special directives at the start of your script file to give the same information to PBSPro, which is the way we recommend doing it.

You can observe the jobs you have queued (waiting for execution) or running, you can use the `qstat` command. This will show every job running on the system. In order to filter down to only your jobs, you can do the following:
`qstat -u $USER`

As part of the contents of your script, or on the qsub command itself, you must provide some information to the system as to how much resource your job will need while it is running. The primary resources you will need to specify are number of CPUs, amount of memory and the elapsed time the job will need to run. It is important that the information you give is reasonably accurate. If the job uses more than you asked for, then the system will terminate the job. If you ask for a lot more than you need, then your job may well be delayed waiting to start because there were not enough resources available at the time.

## Very simple PBSPro script

```bash
#!/bin/bash
#PBS -l select=1:ncpus=1:mem=1gb
#PBS -l walltime=00:01:00
#PBS -N hello_world
 
cd $PBS_O_WORKDIR
 
echo "Hello, world!"
```

Let's break down this script line by line:

1. `#!/bin/bash`: This is the shebang line that specifies the path to the Bash interpreter.
1. `#PBS -l select=1:ncpus=1:mem=1gb`: This line specifies the resources required for the job. In this case, we request a single node with one CPU and 1GB of memory
1. `#PBS -l walltime=00:01:00`: This line specifies the walltime limit for the job, which is one minute in this case.
1. `#PBS -N hello_world`: This line specifies the name of the job.
1. `cd $PBS_O_WORKDIR`: Change to the directory this job was submitted from. Output logs should also appear in this directory

To submit this job to PBSPro, you can use the qsub command followed by the name of the script file. For example:

```console
qsub hello_world.pbs
```

It will return a JobID which you can use to keep track of your job or to delete it.

## Deleting Jobs
You can delete any jobs from PBSPro either before it has run or while it is running. 

First, you need to find the ID of the job you want to delete. You can use the qstat command to display a list of all your jobs currently running or queued:

```console
qstat -u $USER
```

This will display a table of all the jobs, along with their status, ID, and other information. Find the ID of the job you want to delete in the first column of the table.

Once you have the job ID, you can delete the job using the qdel command:

```console
qdel <job_id>
```

Replace <job_id> with the actual ID of the job you want to delete. For example:

```console
qdel 12345
```

This will send a request to PBSPro to delete the specified job.

If the job is in a state where it cannot be deleted (e.g., it is running or has already completed), you can use the -Wforce option to force the deletion:

```console
qdel -Wforce <job_id>
```

This will force the deletion of the job, even if it is in a state where it cannot be deleted normally. Note that forcing the deletion of a running job can cause data loss or other issues, so use this option with caution.
