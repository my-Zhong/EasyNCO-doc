# [POMO](https://proceedings.neurips.cc/paper/2020/hash/f231f2107df69eab0a3862d50018a9b2-Abstract.html)

the proposed Light Encoder and Heavy Decoder (LEHD) method is a neural solver for Vehicle Routing Problems that 
integrates a primary global policy, which learns from the complete VRP instance, with an auxiliary local policy that
learns from transferable local topological features to boost generalization performance.

```bib

```
## POMOInitialization

**Bases:** `Initialization`

**Methods:** `POMOInitialization`

The inputs consist of a tensor containing (x, y) coordinates of the instance. POMO then construct a routing solution step 
by step. After POMOInitializaiton, it outputs a solution solved by the model.




## Usage
```bash
python train.py setting=pomo_settings mode=train problem={problem}
 
```

## Task

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)


## Policy
+ `class POMOPolicy`
```python
class POMOPolicy(nn.Module):
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

+ **Parameters** of AM
  +  general parameter same as [AM](../methods/am.md)
  + multihead_bias: whether use bias in multi head attention.
  + 
+ Backbone: POMO utilize the [Transformer](../developer_doc/methods.md#transformer) to predict next node.






