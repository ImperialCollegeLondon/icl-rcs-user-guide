# HPC Cluster Specification

!!! info

    This page has been rewritten for CX3 Phase 2.

The RCS Platforms team currently run two HPC clusters, a general purpose cluster available for most research workloads called [CX3](#cx3), and a "capabilty cluster" called [HX1](#hx1) which is designed for multi-node workloads and GPU accelerated jobs requiring more than 48 GB of GPU RAM.

The total facility size (as of March 2025) is: 711 compute nodes, 67,896 cores, 876 TB of RAM and 216 GPUs. Specific details on each cluster are provided below.

## CX3

CX3 is the primary HPC facility at Imperial. It is large-scale facility designed for most computational research workflows. CX3 is currently undergoing a refresh, migrating from the "CX3 Legacy" platform to the "CX3 Phase 2" platform, with an updated operating system and job scheduler. The documentation on this site has been rewritten for CX3 Phase 2, the [CX3 Legacy page](./legacy-systems/cx3-legacy.md) provides advice on using that service if you have not yet migrated.

The hardware listed below is for both "CX3 Legacy" and "CX3 Phase 2" although you should expect all of the hardware is eventually migrated to CX3 Phase 2.

* Compute Nodes
    * 325 x AMD nodes, with 2x AMD EPYC 7742 (128 cores, 1TB RAM per node)
    * 53 x Intel nodes, with 2x Intel Icelake Xeon Platinum 8358 (64 cores, 500GB RAM)
    * 12 x AMD (Large Mem) nodes, with 2x AMD EPYC 7742 (128 cores, 4TB RAM per node)
* GPU Nodes
    * 11 x AMD nodes, with 2x AMD EPYC 7742 (128 cores, 1TB RAM, 8 Quadro RTX 6000 per node)
    * 7 x Intel nodes, with 2x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1TB RAM, 8 L40S 48 GB GPUs per node
    * 2 x Intel nodes, with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1 TB RAM, 2 x A100 40GB GDDR6 GPUs per node
    * 2 x Intel nodes, with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 1 TB RAM, 4 x A40 48GB GDDR6 GPUs per node
* Interconnect: 100GbE
* Storage: Direct access to the Research Data Store

**Total**: 408 nodes, 48,384 cores, 717.5 TB RAM, 56 L40S, 88 Quadro (Turing) RTX 6000

You must be registered in order to access the CX3 facility. Further information on this is provided on the [Get Access](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) page under the [main RCS web pages](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/).

## HX1

HX1 (or Hex) is our production "capability" HPC cluster.

* Compute nodes: Lenovo SD630v2 servers each with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 288 nodes; 18,432 compute cores; 512 GB RAM per node
* GPU nodes: Lenovo servers each with 4 x NVidia A100 80 GB RAM GPUs; 2 x Intel Xeon Platinum 8360Y (Ice Lake) 2.40GHz 36-core processors; 1 TB RAM per node; 15 nodes; 60 GPUs in total
* Interconnect: NVidia ConnectX-6 HDR200 (200 Gbit/s) InfiniBand
* Storage: 2 PB IBM Spectrum Scale (GPFS) on Lenovo DSS-G Storage system. Approximately 300TB of additional solid stage storage

**Total**: 303 compute nodes, 19,512 cores, 159 TB of RAM, 60 A100 GPUs

Further details including how to request access can be found on the [HX1 Cluster page](./hx1.md).