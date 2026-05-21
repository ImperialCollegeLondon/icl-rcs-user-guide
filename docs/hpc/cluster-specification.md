# HPC Cluster Specification

The information on this page was updated on [insert date], 

The RCS Platforms team provides two HPC services:

1. The "capabilty machine", [HX1](#hx1), is designed for inherently multi-node workloads such as simulations, as well as GPU accelerated jobs requiring more than 48 GB of GPU RAM. This service has gated access due to its specialised nature.

2. The general purpose cluster available for most research workloads, [CX3](#cx3), is suited to high throughput processing and 'single-node' jobs. This service also acts as an entry-point and sandbox to people new to engaging with HPC systems.

Each service is known as a "cluster" as it consists of a cluster of nodes. Nodes can be considered in the same manner as a computer (consisting of a variety of CPUs and/or GPUs). In a cluster these nodes, or computers, are connected through networking cables allowing multiple nodes and the CPUs/GPUs contained within, to be used together for workloads that require higher compute power (HX1) or concurrently for workloads requiring the same job to be run multiple times (CX3)

Each node consists of CPUs or GPUs (2 in each of our nodes). Each CPU consists of a number of processor cores which do all the hard work, reading and executing programme instructions. CPUs with multiple cores can run instructions on separate cores at the same time within the same unit, increasing overall speed for high-throughput or capability computing.

## HX1 Hardware

HX1 is a "capability cluster", suited to running complex code with high volumes of data that require multiple CPUs to manage the unique workloads. Because multiple CPUs are required, these workloads are known as "multi-node tasks" due to the need for multiple nodes to process data in tandem with one another.

How to request access along with further details of how to use the service can be found on [the HX1 Cluster page](./hx1.md).

* **Compute Only Nodes**:
  * 288 nodes running 2 Intel Icelake Xeon Platinum 8358 CPUs per node. Each node has 64 cores and 512 GB RAM
* **GPU-fitted Nodes**:
  * 15 nodes running 2 Intel Xeon Platinum 8360Y (Ice Lake) CPUs per node. Each node has 72 cores with 1 TB RAM and 4 NVidia A100 GPUs
* **Networking**:
  * InfiniBand (200 Gbit/s) via NVidia ConnectX-6 HDR200 between each compute node
  * Ethernet (100GbE) between each node and storage
* **Storage**:
  * 2 PB of HDD storage
  * 300TB of SSD storage

**Total**: 303 nodes, 19,512 cores, 159 TB of RAM, 60 A100 GPUs.

## CX3 Hardware

CX3 is the primary HPC facility at Imperial. It is a large-scale facility designed for most computational research workflows, typically those that are high throughput single-node tasks, but is able to handle small multi-node tasks. Please note that wait times for these multi-node tasks are incredibly long as it requires waiting for multiple nodes to become free, thus we recommend utilising the HX1 system where possible for these tasks.

While anyone can use our CX3 serice, you must be a registered user. Further information on this is provided on the [Get Access](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) page under the [main RCS web pages](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/).

!!! info

    The current documentation here is for the latest version of the CX3 service, dubbed "CX3 Phase 2". We do not envisage anyone still using the older system version -- dubbed "CX3 Legacy" -- but if you have been unable to migrate your work, then please see [the advice proivded](./legacy-systems/cx3-legacy.md) for CX3 Legacy.

The hardware listed below is comprehensive for both the Legacy system and for Phase 2, where all CX3 Legacy hardware will eventually be repurposed under the Phase 2 system.

* **CPU Only Nodes**:
  * 325 nodes running 2 AMD EPYC 7742 CPUs. Each node has 128 cores and 1 TB RAM
  * 12 nodes running 2 AMD EPYC 7742 CPUs. Each node has 128 cores and 4 TB RAM
  * 53 nodes running 2 Intel Icelake Xeon Platinum 8358 CPUs. Each node has 64 cores with 500 GB RAM
  
* **GPU-fitted Nodes**:
  * 11 nodes running 2 AMD EPYC 7742 CPUs. Each node has 128 cores with 1TB RAM and 8 Quadro (Turing) RTX 6000 GPUs
  * 11 nodes running 2 Intel Xeon Platinum 8358 (Ice Lake) CPUs. Each node has 64 cores with 1 TB RAM and a set of GPUs; where the GPUs are distributed as follows:
    * 7 nodes with 8 L40S GPUs and 48 GB RAM
    * 2 nodes with 4 A40 GPUs and 48 GB RAM
    * 2 nodes with 2 A100 GPUs and 40 GB RAM
* **Networking**:
  * Ethernet (100GbE) between each node and storage
* **Storage**:
  * Direct access to [the Research Data Store](..\rds\index.md) system

**Total**: 412 nodes, 48,384 cores, 717.5 TB RAM, 156 GPUs (88 Quadro RTX 6000, 56 L40S, 8 A40s, 4 A100s).

