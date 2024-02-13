# PyTorch

[PyTorch](https://pytorch.org/) enables fast, flexible experimentation and efficient production through a user-friendly front-end, distributed training, and ecosystem of tools and libraries.

## Conda
Whether using  CPU's or GPU's, you should install PyTorch into a clean Anaconda environment. You will need to first setup up your own [Anaconda environment](./conda.md).

Please take care when installing PyTorch with other packages. It isn't uncommon for PyTorch to conflict with other packages so we generally recommend keeping your PyTorch environment to a minimum.

```console
module load anaconda3/personal
conda create -n pytorch_env -c conda-forge cudatoolkit=11.8 python=3.11
conda activate pytorch_env
conda install -c "nvidia/label/cuda-11.8.0" cuda-nvcc
python3 -m pip install nvidia-cudnn-cu11==8.6.0.163
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
echo 'CUDNN_PATH=$(dirname $(python -c "import nvidia.cudnn;print(nvidia.cudnn.__file__)"))' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
echo 'export LD_LIBRARY_PATH=$CONDA_PREFIX/lib/:$CUDNN_PATH/lib:$LD_LIBRARY_PATH' >> $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
source $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
python3 -m pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

You should then be able to set this works in a job, for example

```bash
#!/bin/bash
#PBS -lselect=1:ncpus=4:mem=10gb:ngpus=1
#PBS -lwalltime=1:0:0
  
cd $PBS_O_WORKDIR
  
module load anaconda3/personal
source activate  pytorch_env
  
## Verify install:
python -c "import torch;print(torch.cuda.is_available())"
```