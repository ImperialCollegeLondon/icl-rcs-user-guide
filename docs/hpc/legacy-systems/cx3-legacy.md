# CX3 Legacy

!!! info 
	This page contains information regarding the original CX3 "legacy" facility for those users who have not transferred their workflow to the refreshed system yet. CX3 Phase 2 is a refresh of the CX3 platform, with an upgraded operating system and job scheduler. As more users move over to CX3 Phase 2, we will continue to migrate hardware across with the aim to have moved over completely by January 2026.

    The [main documentation](../getting-started/index.md) on this web site has now been rewritten to reflect CX3 Phase 2 and you will find some additional pointers in the [migration to Phase 2](#migrating-to-cx3-phase-2) section of this page.

## Using SSH
### Hostname for SSH
```bash
login.hpc.imperial.ac.uk
```
Do note that due to security concerns, key-based authentication is disabled for the login-nodes. Users will need to login using their college username and password.

### Linux and MacOS
Linux and macOS users have the OpenSSH SSH client automatically installed on their operating system and this can be accessed from a terminal session.

Open a terminal session on your computer and run:
```bash
ssh username@login.hpc.imperial.ac.uk
```
at the command prompt, substituting your own College username and entering your password when prompted.

### Putty

Putty is the SSH client traditionally used by many on Windows and is installed on managed College computers. If you are intending to use Putty on your own (personal or unmanaged) computer, please make sure you install the latest version and only download from [https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html). If you already have Putty installed, please make sure you update your version until at least 0.72.

### Command-line SSH clients

On Linux and macOS you have to use a terminal to connect using a regular SSH client with the "Enable X11 forward" flag "-X".

Your connect command will look like:

```
ssh -X username@login.hpc.ic.ac.uk
```

On other clients you will have to look for the "Enable X11 forwarding" option.

For example on Putty, you will need to navigate to Connection -> SSH -> X11 and activate the "Enable X11 forwarding".

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

## MPI Jobs

An MPI job can be configured with the standard #PBS resource specification:

```bash
#PBS -lselect=N:ncpus=Y:mem=Z
```
and then run with:
```bash
module load mpi
mpiexec a.out
```
This will start NxY MPI ranks, with Y ranks on each of N distinct compute nodes.

### The eternal dilemma, "mpirun" or "mpiexec" ?

The recommendation to use "mpiexec" wrapper or "mpirun" wrapper will depend on what application you are using and what MPI version it was compiled against.

* If you are using any version prior to "mpi/intel-2019.8.254" then the recommendation would be to use "mpiexec". i.e. mpiexec myprogram

* If you are compiling with a newer MPI version you would need to use "mpirun". See section below for details.  i.e. mpirun myprogram


If you have any issues running your program or are not sure what to use, please get in touch with our support team following these guidelines.

### Running a program with mpiexec

mpiexec does not require any arguments other than the name of the program to run. For example:

```bash
mpiexec a.out [program arguments]
```

That being said, you can use the "-n" flag to specify the number of processes. This is useful if you would like to decrease the number of mpi ranks, whilst still selecting the same number of "ncpus" in the jobscript.

```bash
mpiexec -n 8 a.out [program arguments]
```

### Note in particular:

It's not necessary to add "-n" or any other flag to specify the number of ranks. If a specific rank count is required and cannot be exactly specified with N x P, use the optional flag “-n <num>”. The number of ranks requested this way must be no more than N x P
it isn't necessary to specify the full path to the program: mpiexec will search `PATH` for the program

## Conda 
### Anaconda3/personal

Long term users will remember the anaconda3/personal module and the attached documentation. This method relied on packages provided by Anaconda Inc. but in 2020 they changed their terms of service requiring all companies with more than 200 employees to buy a commercial license. As most workflows on HPC use the conda-forge repo anyway we are moving towards using the open source product provided by the same team that manage those repos. Users can continue to use their original environments if they install miniforge but they will have to be activated using the full path. For example, if you installed Tensorflow following the old documentation you can still activate that environment with the following:

```
eval "$(~/miniforge3/bin/conda shell.bash hook)"
conda activate /rds/general/user/username/home/anaconda3/envs/tf2_env
```

If you are still using the old Anaconda3/personal module can we please ask that you [Disable the "defaults" channel](../applications/guides/conda.md#how-to-disable-the-defaults-channel)  as this may require the University to purchase a license for every user. More information on Anaconda's terms of service can be found at: [https://www.anaconda.com/pricing/terms-of-service-faqs](https://www.anaconda.com/pricing/terms-of-service-faqs)

## Jupyter
[JupyterHub](https://jupyter.org/) is a web service enabling multiple users to run their Jupyter Notebooks on shared resources. The Imperial College RCS team provide a JupyterHub service which runs interactive Jupyter notebook sessions on HPC hardware by running them as "batch" jobs. The URL for the JupyterHub service is:

[https://jupyter.rcs.imperial.ac.uk/](https://jupyter.rcs.imperial.ac.uk/)

You must be registered with the HPC service in order to use the JupyterHub service.

## Job Sizing Guidance
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

There is an additional limit of 32 GPUs total per user on the gpu72 queue to allow for fair usage of the GPUs.

### largemem72

There is an additional limit of 16384 GB total per user on the largemem72 queue to allow for fair usage.

### large72

There is an additional limit of 5 running jobs per users on this queue to allow for fair usage. In addition, all nodes requesting 128 CPUS or more will be allocated a whole node as the CPUS from 129-256 are from SMT and are not from physical cores.

Please note there is a default limit of 50 concurrent jobs (queued or running) per user. Some queues may impose additional limits to guarantee fair-share and prevent monopolization.

## Express Access

Standard access to our systems is unmetered and free-at-the-point-of-use. If you find yourself requiring more than the standard service provides, for example to meet a specific deadline, you may pay for express access.

The [Express Access self-service page](https://selfservice.rcs.imperial.ac.uk/express) page is only accessible if you are on the campus network or following the recommended Remote Access guidance.

### Express Access Charges

The current charges for express access may be found on the [Express Access self-service page](https://selfservice.rcs.imperial.ac.uk/express).

### What to know before creating an Express Account

* The "billing run" for express accounts is carried out on the morning of the 10th of each month for the previous month. So the billing run on the 10th September 2022 would be for express usage during August 2022.
* An express account can only be associated with a single funding source. Once the funding source has run out or expired you will no longer be able to use the express account.
* If you make an advanced purchase of credit for an express account and the funding source expires, then you will no longer have access to that advance purchased credit. When this occurs, please contact us so we can transfer any remaining credit to a new express account.

### Creating an Express Account
You can create an express account account on the [Create a new Express Account page](https://selfservice.rcs.imperial.ac.uk/express/new); you will need to select a funding source of which you have access to, and confirm that you authorise payments from that funding source.

Every express account is assigned a corresponding express group name of the form **exp-XXXXX** where **XXXXX** is replaced by a number. This express group name will have a corresponding Linux group created which is used to control access to the express account. Please read through the following sections on how to manage and submit jobs to the express account.

### Submitting jobs using the express account

To submit job to the express queue you must use:

```console
qsub -q express -P exp-XXXXX
```

where **exp-XXXXX** is the name associated with your express account. Note that when you submit a job to the express queue, the job must confirm to one of the job classes as listed on the express self-service page.

### Managing your Express Account

#### Listing your Express Accounts

You can see a list of all the express accounts you have access to by going to the [Your Express Accounts(https://selfservice.rcs.imperial.ac.uk/express/manage/accounts)] page on the self service website.

#### Managing user access to your express account

The following steps describe how to control who can submit jobs against your express account (please note that any changes may take a few hours to reflect on the HPC systems):

* Go to the [Your Express Accounts](https://selfservice.rcs.imperial.ac.uk/express/manage/accounts) page on the self service website so view a list of your express accounts.
* Expand the section belonging to the express account that you wish to update.
* Click on the **Manage user access control group** link for the express account you wish to edit.

You should now be presented with a list of users for the express account.

##### To add a user

* Enter the username of the user in question into the Add user (enter username) text box.
* Click the Add button.

If the username is valid, the page should reload showing a list of users including the newly added on.

##### To remove a user

* Enable the Remove tick box for the user you wish to remove from your express account.
* Click the Update button.

The page should reload showing the list of users minus the ones you've requested are removed.

#### Making an advanced purchase of credit

You can make an advanced purchase of credit for an express access account:

* Go to the [Your Express Accounts](https://selfservice.rcs.imperial.ac.uk/express/manage/accounts) page on the self service website so view a list of your express accounts.
* Expand the section belonging to the express account that you wish to make an advanced purchase for.
* Click the **Make Advance Purchase** link against the corresponding express account.

#### Viewing the usage and billing history of your Express Account

* Go to the [Your Express Accounts](https://selfservice.rcs.imperial.ac.uk/express/manage/accounts) page on the self service website so view a list of your express accounts.
* Expand the section belonging to the express account that you wish to view.
* Click the Usage link to view the usage against the express account.
* Click the Billing History link for a list of charges made against the express account.

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

There are 2 multi-node queues, **capability24** and **capability48**. Capability queues are currently running on AMD Rome with 100 GbE ethernet. To target this system, request either 64 or 128 CPUs and up to 8 nodes. If you are regularly submitting multi-node jobs, please consider applying for access to the [HX1 capability cluster](../hx1.md).

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
                                                                                                             
## Migrating to CX3 Phase 2

CX3 Phase 2 was developed in response to growing issues with the legacy system used to maintain and install the CX3 HPC cluster. This original system, custom-built for Imperial over a decade ago, had become increasingly unreliable and difficult to manage. Its design also made it hard to update the operating system, leading to conflicts between system libraries.

Rather than continuing to patch an outdated and fragile setup, we chose to rebuild the system from the ground up using modern, industry-standard tools. This fresh start allowed us to create a more robust and maintainable installation process for the CX3 cluster.

As part of this overhaul, we also transitioned to a standard version of PBS Pro, moving away from the bespoke variant. This change enables us to receive timely updates and better support for the job scheduler.

The purpose of this section is to highlight changes between CX3 Legacy and CX3 Phase 2 in order to aid your migration between the systems. We will continue to add to this section as more useful information becomes relevant.

### Connecting to CX3

The hostname for connecting to CX3 Legacy is `login.hpc.imperial.ac.uk` whereas on Phase 2 this has become `login.cx3.hpc.ic.ac.uk`. As per CX3 Legacy, this new hostname has multiple login nodes behind it but these now include servers with Intel processors. Please see the [Using SSH](../getting-started/using-ssh.md#hostname-for-ssh) page for more information.

### Batch Jobs

#### Submission scripts

Submission scripts are broadly the same between the Legacy and Phase 2 facilities. However, there are changes in the way that some resource requests work, particularly [MPI jobs](../queues/mpi-jobs.md). We strongly recommend you review those sections of the documentation that relate to [Queuing](../queues/index.md), especially for the types of jobs that you are running.

It may also be important to be aware on the Legacy system, jobs started in the `$TMPDIR` directory (local to the compute node), whereas on the Phase 2 system they start in your `$HOME` directory. You may need to change directories or paths within your submission script to account for this difference.

#### Queues

There have been some changes to the queuing structure, which can be viewed on the [Job Sizing guidance](../queues/job-sizing-guidance.md) page.

#### Running Environment

PBSPro on Phase 2 has much better control of the job environment and tracking of resource usage meaning that you may find your jobs running out of memory on Phase 2 whereas on the Legacy system they did not. We recommend you start with the same resource request as for the Legacy system and make adjustments on Phase 2 as required.

#### Monitoring your jobs

Running `qstat` on the login nodes will now show you all jobs running on the cluster rather than just your own. You can limit qstat to showing just your own jobs by providing your username as an option to `qstat` i.e.

```console
[user@login ~]$ qstat -u user
```

### Scientific applications

Over the past few years, we’ve transitioned from using an ad-hoc approach to centrally installing software to adopting the EasyBuild software installation system. If you're already familiar with [modules installed via EasyBuild](../applications/easybuild.md), the module structure in Phase 2 will look familiar. However, unlike before, you no longer need to manually load the "production" EasyBuild modules using `module load tools/prod` command — these are now automatically loaded on Phase 2 when you login.

If you are using one of the "older" modules, we recommend you use the [module tool](../applications/index.md) to search for modules providing the software and/or libraries that you need. If you are unable to find the software you require, please [contact us](../../support/index.md) to discuss it further.