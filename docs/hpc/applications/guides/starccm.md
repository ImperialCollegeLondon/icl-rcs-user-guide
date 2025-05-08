# StarCCM

!!! info

    This page **has** been rewritten for CX3 Phase 2. Not many changes needed

Please note that Imperial does not have a site license for StarCCM. Users will need to contact their school to obtain the license server information.

## Simple example

Example job script using the airfoil simulation. 

```bash
#!/bin/bash
#PBS -l walltime=2:00:00
#PBS -lselect=1:ncpus=32:mem=100gb
#PBS -N starccm_example
 
module load tools/prod
module load STAR-CCM+/19.02.009-r8
export CDLMD_LICENSE_FILE=<Your license server>
 
cd $PBS_O_WORKDIR
 
starccm+ -np 32 -batch airfoil_bl.sim -machinefile $PBS_NODEFILE
```

### IPv6 networking

CX3 Phase 2 and [HX1](../../hx1.md) are IPv6 only so a few changes need to be made to the job script to enable StarCCM+ to run. An updated version of the simple script above that includes these change would be:


```bash
#!/bin/bash
#PBS -l walltime=2:00:00
#PBS -lselect=1:ncpus=32:mem=100gb
#PBS -N starccm_example
 
module load STAR-CCM+/19.04.007-r8
export CDLMD_LICENSE_FILE=<Your license server>

export I_MPI_HYDRA_BOOTSTRAP="rsh"
export I_MPI_HYDRA_BOOTSTRAP_EXEC="/opt/pbs/bin/pbs_tmrsh"
export I_MPI_HYDRA_BRANCH_COUNT=0
 
cd $PBS_O_WORKDIR
 
starccm+ -mpi intel -mpiflags "-v6" -np 32 -batch airfoil_bl.sim -machinefile $PBS_NODEFILE
```
