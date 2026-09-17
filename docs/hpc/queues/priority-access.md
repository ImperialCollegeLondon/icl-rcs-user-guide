# Priority Access

The paid service on RCS clusters has moved to a reservation model but still charges as the rates given in https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/charging-structure/. This applies to all clusters and a reservation can be requested on any of them.
A reservation model means we create a reservation queue for some amount of compute for a fixed period. We don't charge for the usage of that compute and the normal limits aren't enforced (like walltime) but when the reservation period ends all running jobs will also end. We recommend ensuring jobs are ready before the period begins so it can be utilised correctly.

The amount of resource that can be reservered depends on how busy the normal queues are during the requested period. This is to ensure we are being fair to all of our researchers. Very large requests may have to be reviewed by [Academic leadership team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/about/operational-structure/)

If you would like to request a reservation please raise a [General Research Computing Services (RCS) questions or requests](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/) including the amount of compute needed (ncpus, ngpus, total walltime) and the ICL budget code. We only access ICL budget codes and you must either be the owner of the code or we will request permission from them before we create the reservation

## Using a reservation on PBSPro clusters
CX3 and HeX1 using PBSPro as the job scheduler so when a reservation is created it will look like a new queue with a name begining with "R" followed by a series of numbers. This queue name will be given to you when your reservation is created and in the example below we will assume the it to be "R1234".

### Checking jobs in a reservation
Similar to before you can list all jobs in the reservation with:
```bash
qstat R1234
```
### Submitting a new job
Create the job script as you normally would and when ready submit it with:
```bash
qsub -q R1234 jobscript.sh
```

### Check reservation state
Reservations will start and end at fixed times. Jobs can often be queued up before the start but must finish before the reservation ends.
The state of the reservation can be checked with:
```bash
pbs_rstat R1234
```