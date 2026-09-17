# Priority Access

The paid service on RCS clusters has moved to a reservation model but still charges as the rates given in https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/charging-structure/
A reservation model means we create a reservation queue for some amount of compute for a fixed period. We don't charge for the usage of that compute and the normal limits aren't enforced (like walltime) but when the reservation period ends all running jobs will also end. We recommend ensuring jobs are ready before the period begins so it can be utilised correctly.

The amount of resource that can be reservered depends on how busy the normal queues are during the requested period. This is to ensure we are being fair to all of our researchers. Very large requests may have to be reviewed by [Academic leadership team](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/about/operational-structure/)