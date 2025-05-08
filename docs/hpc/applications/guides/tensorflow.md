# TensorFlow

!!! info

    This page has not yet been rewritten for CX3 Phase 2.

[TensorFlow](https://www.tensorflow.org/) is an open source software library for high-performance numerical computation that allows users to create sophisticated deep learning and machine learning applications. The flexible architecture of this library enables you to deploy computation to one or more CPUs or GPUs. TensorFlow also includes [TensorBoard](https://www.tensorflow.org/guide/summaries_and_tensorboard), a data visualization toolkit.

There are a number of methods that can be used to install TensorFlow, such as using pip to install the wheels available on PyPI, or using conda. 

However, neither install the software optimised for our computing hardware and such might lead to performance penalties. Furthermore, although Tensorflow also works in a CPU mode only, it is not recommended to do so at the performance is massively inferior compared with a GPU installation.

With the move to use EasyBuild as a software management tool, we now can provided architecture specific GPU compiled versions of Tensorflow where the user only needs to load the module and does not require any further installation steps to be done. We leave the now outdated advice about the use of conda for documentation purposes for now.

## Easybuild (preferred)

In the submission script, the following module commands, and only these, need to be used:

```console
module purge
module load tools/prod
module load TensorFlow/2.15.1-foss-2023a-CUDA-12.1.1
```

This will add the latest currently installed version of Tensorflow (2.15.1) using the foss-2023a toolchain (basically compilers etc.), with CUDA version 12.1.1. Newer versions can be installed upon request. Below is an example submission script using this version of Tensorflow:

```bash
#!/bin/bash
#PBS -l select=1:ncpus=4:mem=24gb:ngpus=1
#PBS -l walltime=01:00:00
 
module purge
module load tools/prod
module load TensorFlow/2.15.1-foss-2023a-CUDA-12.1.1
 
# Your python code utilising tensorflow e.g.
python tf2-benchmarks.py --model resnet50 --enable_xla --batch_size 256 --num_gpus 1
```

Further advice on submitting jobs to GPU nodes may be found on our [GPU Jobs page](../../queues/gpu-jobs.md).

## Conda
Whether using CPU's or GPU's, you should install TensorFlow into a clean conda environment. You will need to first setup up your own [conda environment](./conda.md).

Please take care when installing Tensorflow with other packages. It isn't uncommon for Tensorflow to conflict with other packages so we generally recommend keeping your Tensorflow environment to a minimum. 

If you want to have both Pytorch and Tensorflow in your single environment, please follow the section on [PyTorch](./pytorch.md/#tensorflow-and-pytorch)

The example below installs both the CPU and GPU version of Tensorflow. If you don't need/want to use a GPU the you don't need to install Cudatoolkit or cudnn. This installation method is based on the advice at [https://www.tensorflow.org/install/pip](https://www.tensorflow.org/install/pip) You should be able to copy+paste the sets one-by-one. 


```console
conda create -n tf2_env -c conda-forge cudatoolkit=11.8 python=3.11
source activate tf2_env
conda install -c "nvidia/label/cuda-11.8.0" cuda-nvcc
python3 -m pip install nvidia-cudnn-cu11==8.6.0.163
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/:$CUDNN_PATH/lib' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
python3 -m pip install tensorflow==2.12.*
```

## Check TensorFlow Installation

Once the installation has been completed a test job can be submitted to PBS.

```bash
#!/bin/bash
#PBS -lselect=1:ncpus=4:mem=10gb:ngpus=1
#PBS -lwalltime=1:0:0
 
cd $PBS_O_WORKDIR
 
eval "$(~/miniforge3/bin/conda shell.bash hook)"
source activate tf2_env
 
## Verify install:
python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

This job should return a single GPU. When running GPU accelerated TensorFlow, please read more on [GPU jobs](../../queues/gpu-jobs.md).

### CPU job

When running a CPU job make sure that you set `ompthreads` to the number of `ncpus` in the PBS resource selection so that all CPUs are used:

```bash
#PBS -l select=N:ncpus=X:mem=Y:ompthreads=X
```

Finally, consider setting up multiple [conda environments](./conda.md) and compare the performance of different TensorFlow versions.
