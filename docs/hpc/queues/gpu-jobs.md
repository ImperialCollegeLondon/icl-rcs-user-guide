# GPU Jobs

## Submitting GPU Jobs
The current GPU's we have available in the cluster are Nvidia RTX 6000's; if you require newer GPUs, please consider moving to [cx3-phase-2](../pilot/cx3-phase2.md). To request a GPU, you need to use both the qsub flag `ngpus` (to request the desired number of gpus) and the qsub flag `gpu_type` (to select the type of GPU). e.g.;

```bash
#PBS -l select=1:ncpus=4:mem=24gb:ngpus=1:gpu_type=RTX6000
```

The value of `ngpus` is per node. You can select up to 8 GPU's per node. If you request 4 nodes (`select=4`) and 8 GPU's (`ngpus=8`) that will equate to 32 GPU's. 

!!! warning

    Please only request more than 1 GPU if you know your application supports this and you have made the necessary changes to utilise this; some applications need extra steps in order to use more than 1 GPU!
        
Within the context of the running job, the shell environment variable `CUDA_VISIBLE_DEVICES` will be set automatically with indices of the allocated GPUs. Jobs **must** respect this setting, or they will interfere with other jobs co-located on the execution node.

### Job Limits
All GPU jobs currently run via the **gpu72** queue. You will find up to date job limits for this queue on the [Job sizing guidance page](./job-sizing-guidance.md).

### GPU Node Specification

| Node type | Type of GPU | No. of GPUS per node | No. of CPUs | RAM |
| --------- | ----------- | -------------------- | ----------- | --- |
| AMD Rome | RTX6000 | 8 | 256 | 960GB |

## GPU Specification
The details of the different GPU types are:

| GPU Type | Single Precision<br>TFLOPS | Double Precision<br>TFLOPS | Memory<br>GB | Memory Bandwidth<br>GB/s | CUDA Compute Capability | GPU Architecture |
| -------- | -------------------------- | -------------------------- | ------------ | ------------------------ | ----------------------- | ---------------- |
| RTX6000* | 16.3 | <1 | 24 | 670 | 7.5 | Turing |

\* Note that there are two versions of the RTX 6000, one based on the Turing architecture and one based on the Ada architecture, the RTX 6000s deployed at Imperial are the earlier Turing based RTX 6000s.