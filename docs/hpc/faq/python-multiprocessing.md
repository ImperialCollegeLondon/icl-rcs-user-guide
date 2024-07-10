# Python multiprocessing or memory issues

We have encountered a few issues where users try to utilize parallel programming in Python by making use of the [multiprocessing package](https://docs.python.org/3/library/multiprocessing.html).

!!!note

    Please note that this issue is limited to **CX3 phase 1** cluster only.

Let us assume that you requested the following resources in your job script:-

```bash
#PBS -l select=1:ncpus=4:mem=16gb
```

In many cases, the multiprocessing package tries to utilize more memory (much higher than `16 GB` requested) or it tries to use more number of cores than what was actually requested through the scheduler. In many cases, the code starts using almost all the available cores on the processor. In such cases, the scheduler may kill the job sometimes. In other cases, the program may continue to run but the performance may degrade significantly.

In almost all cases, the issue is not with the user's job. It is the configuration of the old PBS on our CX3 cluster.

For **CX3 phase 2** we have upgraded PBS to a newer version which does not cause any such issues. For more details on how to use **CX3 phase 2**, please see https://icl-rcs-user-guide.readthedocs.io/en/latest/hpc/pilot/cx3-phase2/.

Please note that the **CX3 phase 2** is connected with RDS and you can access all your data, environment and software installations as you would do it on **CX3 phase 1**. The only difference is you need to login to different login nodes as mentioned in the above link.

If you are facing any issues with the multiprocessing package on **CX3 phase 1**, we would recommend you to use **CX3 phase 2**. If you are facing any issues with the multiprocessing package on **CX3 phase 2**, please [Contact RCS team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/) and we will try to help you out.
