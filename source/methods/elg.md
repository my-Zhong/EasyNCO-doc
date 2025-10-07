# [ELG](https://arxiv.org/abs/2308.14104)

the proposed Ensemble of Local and Global policies (ELG) method is a neural solver for Vehicle Routing Problems that 
integrates a primary global policy, which learns from the complete VRP instance, with an auxiliary local policy that
learns from transferable local topological features to boost generalization performance.

```bib
@article{gao2023towards,
  title={Towards generalizable neural solvers for vehicle routing problems via ensemble with transferrable local policy},
  author={Gao, Chengrui and Shang, Haopu and Xue, Ke and Li, Dong and Qian, Chao},
  journal={arXiv preprint arXiv:2308.14104},
  year={2023}
}
```

## ELGInitialization

**Bases:** `Initialization`

**Methods:** `ELGInitialization`

The inputs consist of a tensor containing (x, y) coordinates of the instance. ELG then construct a routing solution step 
by step. After ELGInitializaiton, it outputs a solution solved by the model.



## Usage
```bash
python train.py setting=elg_settings mode=train problem={problem} local_enable={local_enable} local_size={local_size} xi={xi} euclidean={euclidean}
python train.py settings=elg_settings mode=train problem=tsp local_enable=True local_size=30 xi=-1 euclidean=False
python train.py settings=elg_settings mode=train problem=cvrp local_enable=True local_size=40 xi=-1 euclidean=False

```

| problem | local_enable | local_size | xi | euclidean |
|:-------:|:------------:|:----------:|:--:|:---------:|
|   tsp   |     True     |     30     | -1 |   False   |
|  cvrp   |     True     |     40     | -1 |   False   |


## Task

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)


## Policy
+ `class ELGPolicy`
```python
class ELGPolicy(nn.Module):
    def __init__(self,
                 env_name: str = "tsp",
                 embed_dim: int = 128,
                 num_heads: int = 8,
                 qkv_dim: int = 16,
                 num_encoder_layers: int = 6,
                 normalization: str = "instance",
                 feedforward_hidden: int = 512,
                 logit_clipping: float = 10,
                 use_graph_mean: bool = False,
                 am_mode: bool = True,
                 first_placeholder: bool = True,
                 local_enable:bool = False,
                 local_size: int = 30,
                 xi: int = -1,
                 euclidean: bool = False,
                 )
```

+ **Parameters** of ELG
  +  general parameter same as [POMO](../methods/pomo.md)
  + `local_enable`: Enable the local policy
  + `local_size`: Number of node considered by the local policy
  + `xi`: Coefficient of distance penalty
  + `eucliedean`: Whether normalize the distance in the neighborhood
+ Backbone: ELG utilize the [Transformer](../developer_doc/methods.md#transformer) to predict next node.



## Training

ELG applies autoregressive reinforcement learning [`calss ARREINFORCELightning`]



