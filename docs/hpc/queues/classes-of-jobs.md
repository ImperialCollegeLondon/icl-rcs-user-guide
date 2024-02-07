# Classes of Jobs

With the latest [job sizing guidance](./job-sizing-guidance.md), it is no longer necessary to know which queue to send your job to as this will automatically be determined by the job scheduler. However, it can still be useful to understand the different classes of jobs that may typically be run on an HPC system such as that provided by Imperial.

## High-Throughput

This job class typically refers to a high number of relatively small jobs. Ideally these jobs should fit within a single compute node. Often these jobs will utilise a single CPU core or small number of CPUs.

## High-End

This job class refers to those jobs that are typically requesting whole compute nodes to multiple compute nodes. They may utilise a mixture of technologies such as OpenMP for "threading" inside compute nodes and MPI for communication between compute nodes. 

## Capability

This job class typically refers to those jobs that are run across many compute nodes (often 10 or more), normally using a technology such as MPI to communicate between them. Jobs such as this will take advantage of any fast and low latency interconnect between compute nodes such as Infiniband.

## Large Memory

These are jobs that require large amounts of memory in order to run. For Imperial College, this currently refers to compute jobs requiring more than 920GB of RAM.

## GPU
The GPU class of job refers to those jobs/applications that are utilising GPU technologies such as NVidia's CUDA to run computational work on GPU accelerator cards. See the section on GPUs in the [Job sizing guidance](./job-sizing-guidance.md) for further information.