# CX3 Legacy

CX3 Phase 2 is a refresh of the CX3 platform, with an upgraded operating system and job scheduler. This page contains information regarding the origin CX3 "legacy" facility for those users who have not transferred their workflow to the refreshed system yet. 

## Classes of jobs

With the latest [job sizing guidance](#job-sizing-guidance), it is no longer necessary to know which queue to send your job to as this will automatically be determined by the job scheduler. However, it can still be useful to understand the different classes of jobs that may typically be run on an HPC system such as that provided by Imperial.

### High-Throughput

This job class typically refers to a high number of relatively small jobs. Ideally these jobs should fit within a single compute node. Often these jobs will utilise a single CPU core or small number of CPUs.

### High-End

This job class refers to those jobs that are typically requesting whole compute nodes to multiple compute nodes. They may utilise a mixture of technologies such as OpenMP for "threading" inside compute nodes and MPI for communication between compute nodes.

### Capability

This job class typically refers to those jobs that are run across many compute nodes (often 10 or more), normally using a technology such as MPI to communicate between them. Jobs such as this will take advantage of any fast and low latency interconnect between compute nodes such as Infiniband.

### Large Memory

These are jobs that require large amounts of memory in order to run. For Imperial College, this currently refers to compute jobs requiring more than 920GB of RAM.

### GPU
The GPU class of job refers to those jobs/applications that are utilising GPU technologies such as NVidia's CUDA to run computational work on GPU accelerator cards. See the section on GPUs in the [Job sizing guidance](#job-sizing-guidance) for further information.

### Job Sizing Guidance
The following queues of jobs are supported:

| Queue | Use Cases | Nodes per job | No. of cores per job<br>(ncpus) | Mem<br>(GB) | Walltime<br>(hrs) |
| ----- | --------- | :-------------: | ---------------------------- | -------- | -------------- |
| short8 | Short running jobs | 1 | 1 - 256 | 1 - 920 | 0 - 8 |
| [pqjupyter](#pqjupyter) | Queue for JupyterHub jobs* | 1 |1, 2, 4, 8 | 8, 16, 32, 64 | 8 |
| [pqood](#pqood) | Queue for Open OnDemand (RStudio) jobs* |1 |1, 2, 4, 8 |8, 16, 32, 64 | 8 |
| throughput72 | Low core jobs | 1 | 1- 8 | 1 - 100 | 9 - 72 |
| [medium72](#medium72) | Single-node jobs | 1 | 9 - 127 | 1 - 920 | 9 - 72* |
| [large72](#large72) | Whole node jobs | 1 | 128 - 256 |1 - 920 | 9 - 72* |
| [largemem72](#largemem72) | Large memory jobs* | 1 | 1 - 256 | 921 - 4000 | 0 - 72 |
| [gpu72](#gpu72) | Main queue for gpu job* | 1 - 4 | 1 - 256 | 1 - 920 | 0 - 72 |
| capability24 | Multi-node jobs 24h |2 - 8 | 64 - 128  | 1 - 920 |     0 - 24 |
| capability48 | Multi-node jobs 48h | 2 - 8 | 64 - 128 | 1 - 920 | 24 - 48 |
