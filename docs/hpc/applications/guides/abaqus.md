# Abaqus

!!! info

    This page **has** been rewritten for CX3 Phase 2. (Still all relevant)

Several versions of [Abaqus](https://www.3ds.com/products-services/simulia/products/abaqus/) are installed on our systems, use the command `module spider abaqus` to see the exact versions available.

## Licensing

Abaqus is a commercial product and requires you to have a suitable license entitlement before you can run. Jobs must be explicitly configured to point to the license server that hold your license entitlement. In the job script add the line: 

```bash
export LM_LICENSE_FILE=port@hostname
```

before running Abaqus. Your group leader will know the correct `port@hostname` details for their licenses.

If you have no license entitlement, licenses can be purchased from the [ICT Technology Store](mailto:techstore@imperial.ac.uk).

Please note that RCS do not provide any licenses for Abaqus.

## Checkpointing your work

Abaqus has a built-in checkpoint and restart feature. This allows you to save your work as you go allow and to restart if a job fails or over runs. To enable checkpointing add the following to your input file:

```
*RESTART, WRITE, OVERLAY, FREQUENCY=10
```

`OVERLAY` maintains a single copy and overwrites it each save. `FREQUENCY=10` saves the restart information every 10 time steps. Please ensure you are saving at a reasonable rate partially if you have a large simulation as it could strain the RDS.

To restart the job, create a new input file newJobName with only a single line:

```
*RESTART, READ
```

Then run Abaqus specifying both the new and old job names:

```bash
abaqus jobname=newJobName oldjob=oldJobName
```

Please see Abaqus documentation for more details

## Analyse and visualise results

Once you have your results we recommend using ABAQUS on your local machine instead of using X-forwarding to open a window from the cluster. You should be able to get ABAQUS 2024 (Teaching License) via SoftwareHub from which you can open the CAE.  After [mounting the RDS](../../../rds/access/index.md) on local you can then open the .odb file as normal. 
