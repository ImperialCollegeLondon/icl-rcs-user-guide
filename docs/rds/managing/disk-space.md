# Managing RDS Disk Space

Quota limits are in place on the RDS that control the amount of disk space used, and the number of files and directories created. These limits are in place to ensure the RDS is fairly used between users (for HPC usage) and so that projects may control their expenditure.

## How do I check my quota?
### RDS Project Allocations

We recommend you use the [self-service website](https://selfservice.rcs.imperial.ac.uk/) to check the usage in your project allocation. From the self-service website, go to "Research Data Store" and then "Your Project Allocations". Expand the list that is shown and click the "Usage" link under the corresponding RDS project. The usage shown via this link is calculated every night by running the du or "disk usage" command under your project directory and will give you the most accurate estimate of your disk space usage.

### HPC User Accounts
When you log into a login node, you will see your disk usage in your Home and Ephemeral spaces presented to you automatically, for example:

```console
 RDS usage at 14:10 on 1/8/2022
 
 Project allocations
 
  rds-project-01
   Live       Data:  2GB of 10.00TB (0%)
              Files: 3k of 10.00M (0%)
   Ephemeral  Data:  1GB
              Files: 0k
   Archived   Data:  0GB of 1000GB (0%)
              Files: 0k of 100000 (0%)
 
 Individual allocation /rds/general/user/username
 
   Home       Data:  52GB of 1.00TB (5%)
              Files: 255k of 10.00M (3%)
   Ephemeral  Data:  1GB of 109.95TB (0%)
              Files: 0k of 20.97M (0%)
```

These values are calculated by the RDS quota system and are updated every hour. You will additionally an approximation of the disk usage in any RDS project you a member of although we recommend you use the self-service website for more accurate usage.

You can see this information at any time by running the `quota` command at the command prompt.

## What happens when I reach my quota limit?

When you reach your quota limit, it will appear as though you are writing to a computer disk that is full. This means:

* every attempt to write a file or create a directory will fail
* you may not be able to log into the HPC login nodes
* your compute jobs may fail as they are unable to write data

Your first step, if you are still able to, is to remove/delete any files you no longer need so as to bring you under your quota limit. If you are unable to do this, please go to [Support and Training](../../support/index.md) follow the details for raising a ticket with us to explore options.

## How do I increase my quota?
### RDS Project Allocations

Only the project owner, which normally is the person that created the project, can increase or decrease the quota of a project using the self-service website.

Go to "Your Project Allocations" on the self-service website, expand the project you wish to change the quota on, and click the "Edit Details" link. From the form that appears, you can change the "Live Quota" and "Archive Quota" values for your project (in GB). Once changed, click the "Update Details" button. Please note that it may take a few hours for the changes to become apparent on the RDS.

!!! warning

    While RDS project allocations are charged by their usage and not their quota, increasing the quota could result in your RDS project being charged for usage if you use more than the free allocation (see [https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/rds/](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/rds/)).

### HPC users personal space ($HOME)

We don't, as a rule, permit increases of quotas for user home space. For **temporary** large usage of data, we expect users to use the Ephemeral space, for longer term usage, users should apply for an RDS project space.