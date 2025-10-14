# LEHD

the proposed Light Encoder and Heavy Decoder (LEHD) method is a neural solver for Vehicle Routing Problems.

## LEHDInitialization

**Bases:** `Initialization`

**Methods:** `LEHDInitialization`

The inputs consist of a tensor containing (x, y) coordinates of the instance. LEHD then construct a routing solution step 
by step. After LEHDInitializaiton, it outputs a solution solved by the model.

## LEHDIteration

**Bases:** `Iteration`

**Methods:** `LEHDIteration`

The inputs is a initial solution given by any known solver. It then performs Random Re-Construction (RRC) to iteratively
destroy a partial solution and reconnect them to form a new solution, the shorter one will be accepted as the current solution.
After given iteration, it will output the current solution.



## Usage
```bash
python train.py setting=lehd_settings mode=train problem={problem}
 
```

## Task

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)


## Policy
+ `class LEHDPolicy`
```python
class LEHDPolicy(nn.Module):
    def __init__(
        self,
        phase: str = 'train',
        env_name: str = 'tsp',
        layer_num: int = 6,
        node_dim: int = 2,
        embed_dim: int = 128,
        num_heads: int = 8,
        qkv_dim: int = 16,
        feedforward_hidden: int = 512,
        normalization: str = None,
        multihead_bias: bool = False,
        first_mode: str = 'random',
    ):
```

+ **Parameters** of ELG
  +  general parameter same as [POMO](../methods/pomo.md)
  + `local_enable`: Enable the local policy
  + `local_size`: Number of node considered by the local policy
  + `xi`: Coefficient of distance penalty
  + `eucliedean`: Whether normalize the distance in the neighborhood
+ Backbone: ELG utilize the [Transformer](../developer_doc/methods.md#transformer) to predict next node.



## Training

LEHD applies autoregressive reinforcement learning [`calss ARREINFORCELightning`]



