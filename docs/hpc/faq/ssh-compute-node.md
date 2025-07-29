# SSH to a compute node

In some cases, you want to SSH into a compute node to see if your job is running as intended or to debug your code. You can do this by following the steps below.

You would need to know the job ID of your job along with the node name where your job is running. To get the node name, you can use `PBS_NODEFILE` environment variable. You can save the node name to a file by running the following command in your job script:

```bash
echo $PBS_NODEFILE >> my_nodefile.txt
```

Once this command runs, you would have something like this in `my_nodefile.txt`:

```bash
cx3-16-2-2
```

This is the node name where your job is running. Also, you would have your job ID when you submitted your job. If you do not remember your job ID, you can get it by running the command:

```bash
qstat -u $USER

#It may return something like this:
Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
--------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
790768.pbs-7    user_name   v1_smal* trial.sh   24405*   1   8   64gb 23:59 R 11:11
```

Once you have the above information, you can SSH into the compute node by running the following command:

```bash
export PBS_JOBID=your_job_id #For example 790768

ssh cx3-16-2-2
```

Once on the compute node, you can check the status of your job or debug your code as needed. You can also use `top` or `htop` to see the processes running on the node.
