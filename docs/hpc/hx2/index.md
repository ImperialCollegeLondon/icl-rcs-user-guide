# HeX2

HX2 (or Hex 2) is the new high throughput Cluster at Imperial designed for the running a large number of jobs or smaller scale AI workflows.

## Cluster Specification

HX2 has 4 racks of CPU compute giving a total of 27,648 CPUs and 1 rack of GPU for a total of 96 NVidia H200 GPUs.

### CPU node specs
2x Intel(R) Xeon(R) 6960P
2 TB RAM
1x 200GbE

### GPU node spec
4x NVIDIA H200 with NVlink
2x INTEL(R) XEON(R) PLATINUM 8562Y+ 
1.5 TB RAM
2x 200GbE 

Please note that CPUs 1,2 on CPU nodes and CPUs 1,2,62,63 on GPU nodes are assigned to run the WEKA storage and can not be used for jobs.
