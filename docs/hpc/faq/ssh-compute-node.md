# SSH to a compute node

In some cases, you may want to SSH into a compute node to see if your job is running as intended or to debug your code. You can do this by following the steps below.

You would need to know the job ID of your job along with the node name where your job is running. To get these details, you can run the following command:

```bash
$ qstat -nu $USER -1

# This will give you the output in the following format
pbs-7: 
                                                           Req'd  Req'd  Elap
Job ID          Username  Queue    Jobname    SessID  NDS  TSK   Memory  Time  S Time
--------------- -------- -------- ---------- ------   ---  ---   ------  ----- - -----
792950.pbs-7     user   v1_small  trial.sh    36936*   1   8      50gb  18:59 R 16:38 cx3-13-24/3*8
```

The last column is the node name where your job is running. In this case, it is `cx3-13-24`. The part after `/` i.e. `3*8` indicates placement of your code in the NUMA domain and can be ignored for now.

Once you have the above information, you can SSH into the compute node by running the following command:

```bash
$ export PBS_JOBID=your_job_id #For example 792950

$ ssh cx3-13-24
```

Once on the compute node, you can check the status of your job or debug your code as needed. You can also use `top` or `htop` to see the processes running on the node.
