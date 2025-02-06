# Job Sizing Guidance

## Submitting a  job
The recommended method for users to run their applications on the college HPC systems is to use the job scheduler (PBS Pro), which manages all resources. Jobs are submitted using "qsub" command. All jobs require a resource specification that indicates the size of the compute resources required and anticipated runtime. This specification should be in the top of the job script and must include the following fields:

```bash
#PBS -l select=N:ncpus=X:mem=Ygb
#PBS -l walltime=HH:00:00
```

Where N is the number of nodes, and X and Y are the number of cpus and amount of memory per node (RAM). HH is the expected runtime of the job in hours.

There is no need to specify a queue, simply define the job in terms of the resources it requires and the system will run it as soon as suitable resources become available. When more than one queue matches a job specification, PBS will place the jobs in the first queue by alphabetical order.

The following queues of jobs are supported:

| Queue | Use Cases | Nodes per job | No. of cores per job<br>(ncpus) | Mem<br>(GB) | Walltime<br>(hrs) |
| ----- | --------- | :-------------: | ---------------------------- | -------- | -------------- |
| short8 | Short running jobs | 1 | 1 - 256 | 1 - 920 | 0 - 8 |
| [pqjupyter](#pqjupyter) | Queue for JupyterHub jobs* | 1 |1, 2, 4, 8 | 8, 16, 32, 64 | 8 |
| [pqood](#pqood) | Queue for Open OnDemand (RStudio) jobs* |1 |1, 2, 4, 8 |8, 16, 32, 64 | 8 |
| throughput72 | Low core jobs | 1 | 1- 8 | 1 - 100 | 9 - 72 |
| medium72 | Single-node jobs | 1 | 9 - 127 | 1 - 920 | 9 - 72* |
| large72 | Whole node jobs | 1 | 128 - 256 |1 - 920 | 9 - 72* |
| largemem72 | Large memory jobs* | 1 | 1 - 256 | 921 - 4000 | 0 - 72 |
| gpu72 | Main queue for gpu job* | 1 - 4 | 1 - 256 | 1 - 920 | 0 - 72 |
| capability24 | Multi-node jobs 24h |2 - 8 | 64 - 128  | 1 - 920 |	0 - 24 |
| capability48 | Multi-node jobs 48h | 2 - 8 | 64 - 128 | 1 - 920 | 24 - 48 |

\* Please see details for specific queues below as there may be additional restrictions or limitations.

### pqjupyter

This queue is where JupyterHub jobs are run. There is a limit of 1 concurrent job per user.

### pqood

This queue is used by Open OnDemand (RStudio) jobs. There is a limit of 1 concurrent job per user.

### interactive

Unfortunatly it isn't possible to run interactive jobs on CX3 Phase 1. If you need an interactive session for debugging please use Jupyterhub/Open OnDemand or you can check out [cx3-phase2](../pilot/cx3-phase2.md).

### medium72

Job extensions are allowed on the medium queue only for jobs requesting the maximum walltime (72h). This is done via the self-service page. You will only be able to extend jobs when they are within 18 hours of reaching the walltime. You can only extend for an additional 24 hours at a time, up to a maximum of 144 hours. The job extension feature exists to be used sparingly, and isn't something to rely on being available at any particular time.

### gpu72

There is an additional limit of 32 GPUs total per user on the gpu72 queue to allow for fair usage of the GPUs. Please see our GPU Jobs guide for how to run GPU jobs.

### largemem72

There is an additional limit of 16384 GB total per user on the largemem72 queue to allow for fair usage.

### large72

There is an additional limit of 5 running jobs per users on this queue to allow for fair usage. In addition, all nodes requesting 128 CPUS or more will be allocated a whole node as the CPUS from 129-256 are from SMT and are not from physical cores.

Please note there is a default limit of 50 concurrent jobs (queued or running) per user. Some queues may impose additional limits to guarantee fair-share and prevent monopolization. 

### Advanced PBS Flags

PBS supports flags (also referred to as directives) in submission scripts that enable advanced users to give more details about the node(s) your job needs to run on. With the decommission of CX1 and CX2, and with CX3 Phase 1 consisting of mostly the same specification of compute node (AMD Rome, GPFS file system and ethernet interconnect) they now have limited usefulness on the current legacy HPC platform; the table below and examples are therefore provided here for reference purposes.

The [cx3-phase2](../pilot/cx3-phase2.md) facility has an updated set of flags/directives which can be reviewed in the documentation.

!!! warning

    We generally advise PBS flags/directives not to be left in submission scripts unless they are actively being used as they can increase queue times.

| Flag | Value | Description |
| ---- | ----- | ----------- |
| writabletmp | True/False | Some applications need to write to /tmp, (cx3 only) |
| filesystem_type | gpfs/nfs | Target only nodes with given filesystem type |
| cpu_type | rome | Target only nodes with given CPU type |
| gpu_type | RTX6000 | Select a specific type of GPU | 
| interconnect | infiniband/ethernet | cx1/cx3 uses Ethernet while cx2 uses IB | 

#### Flag Examples

##### Example 1

```bash
#PBS -lselect=1:ncpus=12:mem=50gb
#PBS -lwalltime=20:0:0
```

A job with more than 8 CPUs, more than 8 hours walltime and less than 920 GB of memory would be run in the **medium72** queue. 

```bash
#PBS -lselect=1:ncpus=12:mem=50gb:cpu_type=rome
#PBS -lwalltime=20:0:0
```

Setting filesystem_type to GPFS would have the same effect as that is a distinguishing feature between the two queues.

##### Example 2

```bash
#PBS -lselect=2:ncpus=64:mem=50gb
#PBS -lwalltime=20:0:0
```

There are 2 multi-node queues, **capability24** and **capability48**. Capability queues are currently running on AMD Rome with 100 GbE ethernet. To target this system, request either 64 or 128 CPUs and up to 8 nodes. If you are regularly submitting multi-node jobs, please consider applying for access to the [HX1 capability cluster](../pilot/hx1.md).

## Which queue will I end up in?

### Single node job example - CPU

```bash
#PBS -lselect=1:ncpus=x:mem=32gb
#PBS -lwalltime=2:0:0
```

* 8 CPUs or less and 8 hours walltime or less then the job will go to **short8**
* 8 CPUs or less but walltime is more than 8 hours then the job will go to **throughput72**
* more than 8 CPUs and more than 8 hours walltime then job will go to **medium72** (see [Advanced PBS Flags](#advanced-pbs-flags) section above)
* more than 8 CPUs but less than 128 and more than 8 hours walltime then job will go to **medium72**
* more than 127, more than 8 hours walltime then job will go to **large72**

### Single node job example - Mem

```bash
#PBS -lselect=1:ncpus=2:mem=xgb
#PBS -lwalltime=2:0:0
```

* 8 CPUs or less, 8 hours walltime or less then the job will go to **short8**
* 8 CPUs or less but walltime is more than 8 hours while mem is 100 GB or less then the job will go to **throughput72**
* more than 8 CPUs but less than 128, more than 8 hours walltime and more than 100 GB then the job will go to **medium72**
* more than 127, more than 8 hours walltime then job will go to **large72**
* more than 920 GB of mem and the job will go to **largemem72**

### Multinode example

```bash
#PBS -lselect=x:ncpus=2:mem=100gb
#PBS -lwalltime=2:0:0
```
Please request whole nodes when using the capablity queue. If you need to use more than 1 node your job is likely using all of at least 1 resource on the nodes and so you should be requesting the whole node. The simplest method is to request all the CPUs. 

* 64 or less CPUs and 2 or more nodes while walltime is 72 hours or less is not an accepted job format.
* 64 or 128 CPUs, 8 or less nodes while walltime is 48 hours or less then the job will go to **capability48**
* 64 or 128 CPUs, 8 or less nodes while walltime is 24 hours or less then the job will go to **capability24**
