# Installation

We recommend using **PyTorch 2.x** for better GPU memory utilization and training (testing) acceleration. 

Please configure the Python environment according to the following steps:
Install `torch` first, then install the other dependent packages, finally install `torch-geomatic`.

## 1. Install `torch`

The torch version we have installed is 2.0.1, the official instructions to install **torch 2.0.1** are as follows:
```python
pip install torch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2
```

## 2. Install other dependent packages

You can simply install other dependent packages using the following command
```python
pip install -r requirements.txt
```

It includes the following packages:
```python
numpy==1.24.4
matplotlib==3.7.5
scipy==1.10.1
pyrootutils==1.0.4
hydra-core==1.1.1
pytorch-lightning==2.1.0
lightning==2.1.0
pyyaml==6.0.1
tensorboard==2.14.0
tensorboard-data-server==0.7.2
torchrl==0.1.1
tensordict==0.1.2
llvmlite==0.41.1
numba==0.58.1
random-insertion==0.3.0
revtorch==0.2.4
huggingface_hub
datasets
networkx
paramiko==2.8.0
```

## 3. Install `torch-geomatic`

Finally, you can install **torch-geomatic** (a version of **2.5.1** in EasyCO) via the official documentation:
https://pytorch-geometric.readthedocs.io/en/2.5.1/install/installation.html
```python
pip install torch_geometric==2.5.1
pip install pyg_lib==0.3.1 torch_scatter torch_sparse torch_cluster torch_spline_conv -f https://data.pyg.org/whl/torch-2.0.0+cu117.html
```

> Note that the version of **pyg_lib** must be **0.3.1** for prevent the error of 
> ```python
> Encountered error: `/lib/x86_64-linux-gnu/libm.so.6: version `GLIBC_2.29' not found
> ```
> You can specify the version using the command line below.
> ```python
> pip install pyg_lib==0.3.1 torch_scatter torch_sparse torch_cluster torch_spline_conv -f https://data.pyg.org/whl/torch-2.0.0+cu117.html
> ```