# Controlling Job Log Files

By default, when your job runs, PBS directs the Standard Out (stdout) and Standard Error (stderr) from your running processes (see [Standard Streams](https://en.wikipedia.org/wiki/Standard_streams) for more information) to a temporary directory on the local compute node. These files are then automatically copied to the current working directory where you executed the `qsub` command. This page will provide you advice on how to control how and where these files are written, particularly if you need to [monitor the job when it's running](#monitoring-the-job-logs-from-the-login-nodes), or are happy for the job logs to be discarded.

## Combining the standard out and standard error into a single file

By default, PBSPro will create separate standard out and standard error files for each job that has run. You can use the `-j` option to qsub to combine the two into a single file. For example, to combine the standard out and standard error into the standard out file, you can run:

```
[user@login ~]$ qsub -j oe myjob.pbs
```

which can also be added directly into the submission file by adding the following PBS directive:

```
#PBS -j oe
```

## Changing where the log files are written to

You can use the `-o` and `-e` options to `qsub` to change the locations that the standard out and standard error files are written to at the end of the job. For example:

```console
[user@login ~]$ qsub -o /rds/general/user/home/project1/logs_project1/ -e /rds/general/user/home/project1/logs_project1/ myjob.pbs
```

Would write both the standard out and standard error from the job into `/rds/general/user/home/project1/logs_project1/` after the job has run although note that the directory must exist prior to the job running.

This can also go directly in to the job submisson script:

```
#PBS -o /rds/general/user/home/project1/logs_project1/
#PBS -e /rds/general/user/home/project1/logs_project1/
```

## Monitoring the job logs from the login nodes

As mentioned above, by default the job logs are written to a local temporary directory on the compute nodes and then copied to the current working directory where qsub is executed. Sometimes it can be convenient to monitor the job logs while the job is running. You can do that with the `-koed` option to qsub i.e.

```console
[user@login ~]$ qsub -koed myjob.pbs
```

With this option standard out and standard error (`oe`) to be written to their final destination as the job is running so it can be viewed as the job runs. You can add this as a PBS directive to the submission script:

```
#PBS -koed
```

## Discarding job logs

Sometimes you might be running jobs where you don't need to keep the job logs. If you were running this locally on your own computer you might redirect these logs to be sent to /dev/null. You can reproduce the same effect with PBSPro by using a combination of the options already presented on this page, namely `-koed`,  `-o` and `-e`. You can use these options on the command line with qsub to redirect the standard output and standard error to `/dev/null` in the following way:

```console
[user@login ~]$ qsub -koed -o /dev/null -e /dev/null
```

This can be added as a PBS directive in your submission script by adding the following:

```
#PBS -koed -o /dev/null -e /dev/null
```