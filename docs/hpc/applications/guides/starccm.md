# StarCCM

Please note that Imperial does not have a site license for StarCCM. Users will need to contact their school to obtain the license server information.

## Simple example

Example job script using the airfoil simulation. 

```bash
#!/bin/bash
#PBS -l walltime=2:00:00
#PBS -lselect=1:ncpus=32:mem=100gb:writabletmp=true
#PBS -N starccm_example
 
module load star-ccm/16.04.012-R8
export CDLMD_LICENSE_FILE=<Your license server?
 
cd $PBS_O_WORKDIR
 
starccm+ -np 32 -batch airfoil_bl.sim -machinefile $PBS_NODEFILE
```