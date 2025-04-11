# Purpose of the Queuing System

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

## Manage work on behalf of the user
Running work on HPC systems may involve the user in running many jobs, sometimes hundreds or thousands of them. The queuing system provides a mechanism to handle this easily without the need for constant monitoring and attention from the user. The users are able to create and submit standard Linux scripts of any degree of complexity to be run on the system, allowing complete flexibility as to the work which is being run.

The queuing system takes care of starting jobs whenever resources become available, running the work and then delivering the results back to the user. The system automatically manages thousands of CPUs, running a mixture of different size and length of runs from different users. This is all handled transparently and without user intervention.

For those researchers moving from overnight and weekend runs on their own desktop PCs, this means that you no longer have to come into work on Saturday evening to see if a run has finished and start another one. You can submit as much work as you need to at any time and the system will process all the jobs without further intervention from you.

## Efficiently Managing System Resources
With such a large resource with many CPUs and nodes of different memory size and number of CPUs, hit is important to ensure that the machines are used as efficiently as possible, to deliver the maximum possible processing resources to the researcher. The system runs 24 hours a day, 7 days a week. Users submit a wide range of different jobs, ranging from 1 to hundreds of CPUs, with run times from seconds to days. The scheduling system manages all this variety with the aim of running as much work as possible without overloading any part of the system.

## Deliver Site Determined Scheduling Policies
Whilst a prime aim of the scheduler is to ensure the best possible use of the hardware, there are also scheduling policies which are in place with the express aim of providing the users of the system with timely delivery of results in accordance with their expectations. These policies are targeted to fulfil specific user requirements. Examples are to be able to submit short test runs and get rapid results, which is useful for determining parameter settings for applications, or for testing new codes. This includes being able to make quick runs with parallel jobs, not just serial ones.

These scheduling policies are made on the basis of the workload being submitted to the system. As user demands change the policies can be adjusted to match the requirements.