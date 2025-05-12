# Long Queue Times

This is one of the most common questions that come to the RCS team, and it comes in different variations:

1. My job has been queued for longer than it has done in the past.
1. Are there any methods to reduce the wait time for my job?

We would like to point out that the HPC facility provided by the College is a shared facility, like you, there are hundreds of people using the cluster at any given time and thousands (approximately) of jobs running simultaneously on our cluster. There are many factors that decides how much time your job has to wait in the queue such as:-

## How much resource have you asked for?

The more resources (CPU/GPU/RAM) you ask for, the longer the queue time is (generally speaking).

## What is your historical usage of the cluster in the recent past?

Most of the users have some sort of urgency to get their results faster. Because of this, they may submit a large number of jobs or may ask for large amount of resources. Fairshare is a tool utilised by our scheduler (PBS Pro) and ensures that resource is allocated as fairly as possible to all users. This is to prevent monopolisation of the cluster. The PBS scheduler keeps track of your fairshare usage for the last 2 weeks (approximately) and your wait times may increase based on how much you've used the cluster in the recent past.

## The current load on the cluster.

If the cluster is very busy, you may notice an increase in queue times.

## Hardware/service issues

While the RCS team tries its best to keep the service up and running, we may experience some technical issues and have to offline a few nodes for the maintenance. In such cases, your wait time can be higher than the usual you have experienced in the past.

## Scheduling in general

The scheduler on the cluster i.e. PBS is responsible for running you jobs as and when resources are available and the RCS team cannot do much in this regard. The wait time will depend on the factors stated above and as you can imagine, it is a dynamic process. However, please note that the longer your job waits (or has waited in the queue), the probability of your job running next keeps on increasing.

While no user can bypass the queue and get priority over another (Priority Access will be different in this case), you may experience some counter intuitive behavior. Let us suppose that your job is in the queue for 2 days, and your colleague submits a job. Their job queued for only a few minutes before running on the cluster. This happens because schedulers are very efficient in back filling jobs. Back filling in general means scheduling other jobs to use the reserved job slots, as long as the other jobs do not delay the start of another job.

To further elaborate on the above, assume that your friend wanted to run a job on 5 cores for 5 minutes only. While you have asked for 20 cores for 20 minutes. At present, there are only 10 free cores on the system. The scheduler has reserved a place for your job in another 10 minutes as the other 10 cores become free in 10 minutes. This is where the back filling comes in. The scheduler can run your friends job as it can finish in 5 minutes on the number of cores which are currently available and running this job does not delay the start time of your job.

We hope that you find this piece of information interesting and you now have better idea about your longer wait times.
