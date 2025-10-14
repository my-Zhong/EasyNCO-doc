# INViT

the proposed Invariant Nested View Transformer (INViT) method is a neural solver for Vehicle Routing Problems. 

## INViTInitialization

**Bases:** `Initialization`

**Methods:** `INViTInitialization`

The inputs consist of a tensor containing (x, y) coordinates of the instance. INViT then construct a routing solution step 
by step. After INViTInitializaiton, it outputs a solution solved by the model.




## Usage
```bash
python train.py setting=invit_settings mode=test problem={problem}
```

## Task

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)


## Policy
+ `class INViTPolicy`
```python
class INVITPolicy(nn.Module):
    def __init__(
        self,
        # node_dim: int = 2,
        env_name: str = "tsp",
        embed_dim: int = 128,
        num_heads: int = 8,
        qkv_dim: int = 16,
        feedforward_dim: int = 512,
        action_embed_layer_num: int = 2,
        state_embed_layer_num: int = 2,
        decoder_layer_num: int = 3,
        normalization: str = "layer",
        first_placeholder: bool = False,
        state_k: list = [35, 50, 65], # directly from the INViT code
        action_k: int = 15,
        logit_clipping: float = 10
    ):
```

+ **Parameters** of INViT
  +  general parameter same as [POMO](../methods/pomo.md)
  +  action_embed_layer_num: number of layer of action embedding.
  +  state_embed_layer_num: number of layer of state embedding.
  +  state_k: size of K-Nearest-Neighbor (KNN)
+ Backbone: INViT utilize the [Transformer](../developer_doc/methods.md#transformer) to predict next node.



## Training

INViT applies autoregressive reinforcement learning [`calss ARREINFORCELightning`]



