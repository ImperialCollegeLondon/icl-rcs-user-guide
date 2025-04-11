# Data Management on HPC

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

As an HPC user you will have access several different storage spaces

## Storage Spaces on the RDS

Your Home and Ephemeral spaces, as well as any Research Data Store project allocations you have access to, are stored on the Research Data Store; please see the [Research Data Store user guide](../../rds/index.md) for more information on how to manage access, check how much data you are using, and how to access the data from your own computer.

### Home Directory

This is your default working space on the HPC systems and will be your current directory when you login to the HPC facility. Here you have an allocation of 930GB (and up to 10 million files). Your home area will remain for as long as you are a registered user of the HPC service and will be deleted once your are removed from an HPC group or leave the University.

To refer to this location in job scripts use the environment variable `$HOME`.

### Ephemeral Directory

This is additional individual working space where you can temporarily store a large amount of **working** research data, for example as a temporary path for running HPC jobs. Any files in this space are automatically deleted after 30 days and they are not backed up so you should not store any files in the ephemeral directories that you cannot afford to lose. You can refer to this location using the environment variable `$EPHEMERAL`. Home and Ephemeral are accessible on HPC interactive systems and within batch jobs. 

!!! info

    From Tuesday 30th July 2024, per user quotas on the two ephemeral storage areas (ephemeral user and ephemeral project) will be set to 10TB. Users who have a genuine (time-limited) need for more than 10TB as part of an HPC workflow should [raise a ticket](../../support/index.md) to discuss a their requirements.
 
!!! warning

    Please **DO NOT** use ephemeral as a backup storage area or alternative to an RDS Project; ephemeral is meant as a temporary area while running HPC jobs. Users who are observed to use ephemeral in this way (for example in the use of copy scripts) are at risk of having their HPC and/or RDS account suspended.

## Storage Spaces on Compute Nodes

### Temporary Job Space

Each running batch job is allocated temporary working space. This is accessible only to the job and deleted as soon as the job completes. The location of this temporary space is found in the environment variable `$TMPDIR`. It is also the default working directory of a job.  Job temporary storage is normally on a disk directly attached to the compute node assigned to the job. It is best used for jobs that create lots of intermediate files or do random access to files, and so need very fast storage.  There is typically between 10-100GB of space available in `$TMPDIR`.