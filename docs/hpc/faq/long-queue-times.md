# Long Queue Times

This is one of the most common question that comes to the RCS team quit often and in many variations (not an exhaustive list) as described below:-

1. My job has to wait longer as compared to the wait time in the past.
1. The estimated start time keeps on changing.
1. Are there any methods to reduce the wait time for my job?

We would like to point out that the HPC facility provided by the college is a shared facility and like you there are hundreds of people using the cluster at any given time and thousands (approximately) of jobs running simultaneously on our cluster. There are many factors that decides how much time your job has to wait in the queue such as:-

## How much Cpu cores/ Gpu / Ram you have asked for?

The more resources you ask for ---→ the longer is the  wait time.

## What is your historical usage of the cluster in the recent past?

Most of the users have some sort of urgency to get their results faster. Because of this, they may submit a large number of jobs or may ask for large amount of resources. Since there is a fairshare policy implemented by the college that every user is to treated equally, one user cannot be allowed to occupy a significant chunk of the cluster for a long time. This is to prevent monopolization of the cluster. The PBS scheduler keeps track of your fair share usage for the last 2 weeks approximately and your wait times increases based on how much you used the cluster in the recent past.

## The current load on the cluster.

Depending on how many people are using the cluster and how much resources is currently available, your wait time increases with increase in the load on the cluster.

## Actual compute hardware currently available and working in good condition.

While the RCS team tries its best to keep the service up and running, we may experience some technical issues because of which we may have to isolate a few nodes for the maintenance / update or repair. In such cases, your wait time can be higher than the usual you have experienced in the past.

The scheduler on the cluster i.e. PBS is responsible for running you jobs as and when resources are available and the RCS team cannot do much in this regard. The wait time will depend on the factors stated above and as you can imagine, it is a dynamic process. This is why when you first see an estimated start time, it may change later because the scheduler can only provide a rough estimate. However, please note that the longer your job waits (or has waited in the queue), the probability of your job running next keeps on increasing.

While no user can bypass the queue and get priority over another (Express queues are different in this aspect), you may experience one counter intuitive behavior. Let us suppose that your job is in the queue for 2 days (just for example) and your friend submits a job and his/her job waited for only a few seconds before running on the cluster. This happens because schedulers are very efficient in back filling the jobs. Back filling in general means scheduling other jobs to use the reserved job slots, as long as the other jobs do not delay the start of another job.

To further elaborate the above, assume that your friend wanted to run a job on 5 cores for 5 minutes only. While you have asked for 20 cores for 20 minutes. At present, there are only 10 free cores on the system. The scheduler has reserved a place for your job in another 10 minutes as the other 10 cores become  free in 10 minutes. This is where the back filling comes in. The scheduler can run your friends job as it can finish in 5 minutes on the number of cores which are currently available and running this job does not delay the start time of your job.

We hope that you find this piece of information interesting and you now have better idea about your longer wait times.