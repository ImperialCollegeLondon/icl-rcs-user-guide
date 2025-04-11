# How to profile your AI/ML workload using PyTorch

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

If you have an AI/ML workload that uses PyTorch, you can profile it using PyTorch's built-in profiler. The full details can be found on [PyTorch Profiler](https://pytorch.org/tutorials/recipes/recipes/profiler_recipe.html#using-profiler-to-analyze-memory-consumption).

We are writing this page as a general guidance on the most important step only. Rest of details can be found in the above link.

Let us assume that you have a function called  `trainer` in your code which is responsible for training with your dataset. You want to see how much memory is being consumed by this function, what are the various sizes of the tensors being used etc. 

1. First, import the necessary libraries..

```python
import torch
import torchvision.models as models
from torch.profiler import profile, record_function, ProfilerActivity
```

2. Wrap your train function with the profiler as shown below. For brevity, we have not included the full code.

```python
with profile(activities=[ProfilerActivity.CPU, ProfilerActivity.CUDA], with_stack=True, profile_memory=True, record_shapes=True) as prof:
    trainer.fit(model, dataloader_train_MICLe)
```  

where ProfilerActivity.CPU or ProfilerActivity.CUDA can be used to profile activities on CPU and GPU respectively.
If you do not want to see the call stack or you do not want to see the shapes of the tensors, you can skip the options `with_stack=True` and `record_shapes=True` respectively. 

Please note that lesser options you use to profile the code, the faster will be the profiling. However, you will also get less information.

3. To print the results, you can use the commands like 

```python
print(prof.key_averages().table(sort_by="cpu_time_total", row_limit=10))

print(prof.key_averages(group_by_input_shape=True).table(sort_by="cpu_time_total", row_limit=10))

print(prof.key_averages().table(sort_by="cpu_memory_usage", row_limit=10))
```

You can use one of all of the above 3 command depending on what you are looking for. If you want to sort by GPU time or usage, replace `CPU` with `CUDA` in the above commands.

## Other ways to profile your code

1. You can also use Nvidia profiler to profiler your code. For more details, please see the section on [Nsight Systems](../../advanced/profiling/nsight_systems.md) and [Nsight Compute](../../advanced/profiling/nsight_compute.md).

2. If you are only interested to see the memory usage dynamically, your can use `nvitop` tool. For more details, please see the section on [nvitop](../../advanced/profiling/nvitop.md).