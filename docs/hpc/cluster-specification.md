# HPC Cluster Specification

The RCS Platforms team currently run two HPC clusters, a general purpose cluster available for most workloads called CX3, and new cluster called HX1 which is designed for multi-node and larger GPU. In addition, we can provide access to the Jade2 national facility; please raise a ticket by following the guidance on the [Raise a ticket and contact us page](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-support/contact-us/). The total facility size (as of August 2023) is: 661 compute nodes, 64,216 cores, 545 TB of RAM and 228 GPUs. Specific details on each cluster are provided below.

## Where are the details for CX1?

CX1 is the name for the old HPC service, consisting of many compute nodes with a range of specifications, connected over 1GbE. Due to their age, with all of the compute nodes out of warranty by many years (and some in fact unsafe to leave running), most of the CX1 compute nodes have now been decommissioned. The remaining compute nodes will continue to be decommissioned at appropriate times.

## What happened to CX2?

CX2 was the HPC cluster with a fast interconnect (Infiniband) designed for large-scale multi-node jobs. Most of the hardware was very old and a large proportion of it failed over Christmas 2022/2023. It was completely decommissioned in January 2023 to make way for the HX1 cluster which replaces it.

## CX3

CX3 is currently going through a mini-refresh called [Phase 2](./pilot/cx3-phase2.md). The goal is for all the hardware listed below to be moved over to this newer OS and users are free to try this system out now. 

* 325 compute nodes with 2x AMD EPYC 7742 (128 cores, 1TB RAM per node). Racks cx3-1 to cx3-7 and from cx3-12 to cx3-15
* 53 computes nodes with 2x Intel Icelake Xeon Platinum 8358 (64 cores, 500GB RAM). Racks cx3-16 to cx3-19
* 11 GPU nodes with 2x AMD EPYC 7742 (128 cores, 1TB RAM, 8 Quadro RTX 6000 per node). Rack cx3-11
* 7 GPU nodes with 2x Intel Icelake Xeon Platinum 8358 (64 cores, 1TB RAM, 8 L40S 48 GB GPUs per node), Rack cx3-20
* 12 large mem compute nodes with 2x AMD EPYC 7742 (128 cores, 4TB RAM per node). Rack cx3-8 and cx3-10-12
* Interconnect: 100GbE
* Storage: Direct access to the Research Data Store

**Total**: 408 nodes, 48,384 cores, 717.5 TB RAM, 56 L40S, 88 Quadro (Turing) RTX 6000

## HX1

HX1 (or Hex) is our production "capability" HPC cluster. Further details including how to request access can be found on the [HX1 Cluster page](./pilot/hx1.md).

* Compute nodes: Lenovo SD630v2 servers each with 2 x Intel Xeon Platinum 8358 (Ice Lake) 2.60GHz 32-core processors; 64 cores per node; 288 nodes; 18,432 compute cores; 512 GB RAM per node
* GPU nodes: Lenovo servers each with 4 x NVidia A100 80 GB RAM GPUs; 2 x Intel Xeon Platinum 8360Y (Ice Lake) 2.40GHz 36-core processors; 1 TB RAM per node; 15 nodes; 60 GPUs in total
* Interconnect: NVidia ConnectX-6 HDR200 (200 Gbit/s) InfiniBand
* Storage: 2 PB IBM Spectrum Scale (GPFS) on Lenovo DSS-G Storage system. Approximately 300TB of additional solid stage storage

**Total**: 303 compute nodes, 19,512 cores, 159 TB of RAM, 60 A100 GPUs