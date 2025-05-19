# Job Dependencies

PBS allows you to specify dependencies between two or more jobs. Dependencies are useful for a variety of tasks, such as:

Specifying the order in which jobs in a set should execute
Requesting a job run only if an error occurs in another job
Holding jobs until a particular job starts or completes execution

## Limitations of job dependencies

!!! info
	Consider the following circumstance: Job 1 (J1) depends on job 3 (J3) to finish before running. Should job J3 take more than roughly 2 weeks to run (including queue time), J1 may be rejected. This is due to how PBS deals with job history being maintained on CX3 Phase 2.

Job dependencies are supported:

* Between jobs and jobs
* Between job arrays and job arrays
* Between job arrays and jobs
* Between jobs and job arrays

Job dependencies are not supported for sub-jobs or ranges of sub-jobs.

Sub-jobs in arrays run completely independent of each other and in no particular order.

## Syntax for Job dependencies

Use the `-W depend=<dependency list>` option to `qsub` to define dependencies between jobs.

The dependency list has the format: `<type>:<arg list>`

where except for the `on` type, the arg list is one or more PBS job IDs in the form: <job ID>[:<job ID> ...]

The depend job attribute controls job dependencies. You can set it using the qsub command line:

```console
qsub -W depend=...
```

or a PBS directive:

```bash
#PBS -W depend=...
```

## Available Jobs Dependencies

**after: <arg list>**

This job may start only after all jobs in arg list have started execution.

**afterok: <arg list>**

This job may start only after all jobs in arg list have terminated with no errors.

**afternotok: <arg list>**

This job may start only after all jobs in arg list have terminated with errors.

**afterany: <arg list>**

This job may start after all jobs in arg list have finished execution, with or without errors. This job will not run if a job in the arg list was deleted without ever having been run.

**runone: <arg list>**

This command groups jobs so that only one job from the group will be allowed to run, and the rest will be held back.

**before: <arg list>**

Jobs in arg list may start only after specified jobs have begun execution. You must submit jobs that will run before other jobs with a type of on.

**beforeok: <arg list>**

Jobs in arg list may start only after this job terminates without errors.

**beforenotok: <arg list>**

If this job terminates execution with errors, the jobs in arg list may begin.

**beforeany: <arg list>**

Jobs in arg list may start only after specified jobs terminate execution, with or without errors. Requires use of on dependency for jobs that will run before other jobs.

**on:count**

This job may start only after count dependencies on other jobs have been satisfied. This type is used in conjunction with one of the before types. count is an integer greater than 0.

To determine whether a job has run with errors, PBS will use the exit status of the last command executed as the exit status of the job.

## Job Dependency Examples


### Example 1

You have three jobs, job1, job2, and job3, and you want job3 to start after job1 and job2 have ended:

```console
qsub job1
    16394.pbs
 
qsub job2
    16395.pbs
 
qsub -W depend=afterany:16394:16395 job3
    16396.pbs
```

### Example 2

You want job2 to start only if job1 ends with no errors:

```console
qsub job1
    16397.qsub
 
qsub -W depend=afterok:16397 job2
    16396.pbs
```

### Example 3

job1 should run before job2 and job3. To use the `beforeany` dependency, you must use the on dependency:

```console
qsub -W depend=on:2 job1
    16397.pbs
 
qsub -W depend=beforeany:16397 job2
    16398.pbs
 
qsub -W depend=beforeany:16397 job3
    16399.pbs
```

### Example 4

job array 2 should start once job array 1 has completed. Note the square brackets needed for an array job

```console
qsub job_array_1
    17123[].pbs
qsub -W depend=afterany:17123[].pbs job_array2
    17124[].pbs
```
