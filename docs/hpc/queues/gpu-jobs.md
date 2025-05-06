# GPU Jobs

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

## Submitting GPU Jobs
To see the current GPU's we have available in the cluster, take a look at the [following section](#gpu-node-specification). To request any GPU, you need to use the flag `ngpus` (to request the desired number of gpus);

```bash
#PBS -l select=1:ncpus=4:mem=24gb:ngpus=1
```

The value of `ngpus` is per node. You can select up to 8 GPU's per node. If you request 4 nodes (`select=4`) and 8 GPU's (`ngpus=8`) that will equate to 32 GPU's. 

!!! warning

    Please only request more than 1 GPU if you know your application supports this and you have made the necessary changes to utilise this; some applications need extra steps in order to use more than 1 GPU!
        
Within the context of the running job, the shell environment variable `CUDA_VISIBLE_DEVICES` will be set automatically with the UUID of the allocated GPUs. Our systems utilises [Cgroups](https://en.wikipedia.org/wiki/Cgroups), meaning you will only be able to see the resource that is allocated to your job, including GPU. 

## Job Limits
All GPU jobs currently run via the **gpu72** and **gpu72_8** queues. You will find up to date job limits for this queue on the [Job sizing guidance page](./job-sizing-guidance.md).

## Requesting Specific GPU types
In order to request a specific GPU type, you can add the following PBS flag - `gpu_type` 

There are 2 GPU types available in the batch queue: `L40S` and `A100`:
```bash
#PBS -l select=1:ncpus=4:mem=24gb:ngpus=1:gpu_type=L40S
```
or
```bash
#PBS -l select=1:ncpus=4:mem=24gb:ngpus=1:gpu_type=A100
```
Note that we recommend leaving this option empty unless you have a specific need for one or the other.

## GPU Node Specification

|  | [L40S PCIe 48 GB](https://resources.nvidia.com/en-us-l40s/l40s-datasheet-28413) | [A100 PCIe 40GB](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/a100/pdf/nvidia-a100-datasheet.pdf) | [A40 PCIe 48GB](https://images.nvidia.com/content/Solutions/data-center/a40/nvidia-a40-datasheet.pdf) | [RTX6000 PCIe 24GB](https://www.nvidia.com/content/dam/en-zz/Solutions/design-visualization/quadro-product-literature/quadro-rtx-6000-us-nvidia-704093-r4-web.pdf) |
| -------- | -------- | -------- | -------- | -------- |
| FP64 Double Precision (Tensor) TFLOPS | N/A | 19.5  | N/A | N/A |
| TF32 Single Precision (Tensor) TFLOPS | 183  | 156  | 74.8 | N/A |
| FP64 Double Precision TFLOPS | N/A | 9.7  | N/A | <1 |
| FP32 Single Precision TFLOPS | 91.6 | 19.5 | 37.4 | 16.3 |
| Memory | 48 GB GDDR6 | 40 GB GDDR6 | 48 GB GDDR6 | 24 GB GDDR6 |
| Memory Bandwidth GB/s | 864 | 1,555 | 696 | 672 |
| CUDA Compute Capability | 8.9 | 8.0 | 8.6 | 7.5 |
| GPU Architecture | Ada Lovelace | Ampere | Ampere | Turing |
---
| Available in batch queue | Yes | Yes\* | No | No |
| Available in [Jupyterhub](https://jupyterhub-11.rcs.ic.ac.uk/) | No | No | Yes | Yes |

\* Note that there are only a few A100's available in the batch queue, as a result, only request them if you're absolutely sure you need them, otherwise you may be queueing for a long time.
