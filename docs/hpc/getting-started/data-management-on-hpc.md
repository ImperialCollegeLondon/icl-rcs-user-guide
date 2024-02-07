# Data Management on HPC

As an HPC user you will have access several different storage spaces

## Storage Spaces on the RDS

Your Home and Ephemeral spaces, as well as any Research Data Store project allocations you have access to, are stored on the Research Data Store; please see the [Research Data Store user guide](../../../rds/rds-intro/) for more information on how to manage access, check how much data you are using, and how to access the data from your own computer.

### Home Directory

This is your default working space on the HPC systems and will be your current directory when you login to the HPC facility. Here you have an allocation of 930GB (and up to 10 million files). This allocation will remain as long as you are a member of the College and will be deleted when you leave. To refer to this location in job scripts use the environment variable `$HOME`.

### Ephemeral Directory

This is additional individual working space where you can store a very large amount of working research data. Any files in this space are automatically deleted after 30 days. You can refer to this location using the environment variable `$EPHEMERAL`. Home and Ephemeral are accessible on HPC interactive systems and within batch jobs. Please **DO NOT** use ephemeral as a backup storage area; it is meant as a temporary area for active/working research data. Users who are observed to use ephemeral in this way are at risk of having their HPC and/or RDS account suspended.

## Storage Spaces on Compute Nodes

### Temporary Job Space

Each running batch job is allocated temporary working space. This is accessible only to the job and deleted as soon as the job completes. The location of this temporary space is found in the environment variable `$TMPDIR`. It is also the default working directory of a job.  Job temporary storage is normally on a disk directly attached to the compute node assigned to the job. It is best used for jobs that create lots of intermediate files or do random access to files, and so need very fast storage.  There is typically between 10-100GB of space available in `$TMPDIR`.  

