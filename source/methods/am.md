# AM

AM is a neural solver for Vehicle Routing Problems that utilize the transformer architecture and trained by reinforcement learning.


## AMInitialization

**Bases:** `Initialization`

**Methods:** `AMInitialization`

The inputs consist of a tensor containing (x, y) coordinates of the instance. AM then construct a routing solution step 
by step. After AMInitializaiton, it outputs a solution solved by the model.




## Usage
```bash
python train.py setting=am_settings mode=test problem={problem} 
```

## Task

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)


## Policy
+ `class AMPolicy`
```python
class AttentionModelPolicy(nn.Module):
    def __init__(self,
                 env_name: str = "tsp",
                 embed_dim: int = 128,
                 num_heads: int = 8,
                 qkv_dim: int = 16,
                 num_encoder_layers: int = 3,
                 num_decoder_layers: int = None,  # It is None in POMO and POMO_based models
                 normalization: str = "batch",
                 feedforward_hidden: int = 512,
                 logit_clipping: float = 10,  # clipping value for logits, used in Compatibility
                 use_graph_mean: bool = True,  # It is False in POMO and POMO_based models
                 am_mode: bool = True,
                 first_placeholder: bool = True,  # only used in TSP of Attention Model
                 bias: bool = False,  # bias for Wq
                 bias_k: bool = None,  # bias for Wk, if None, use bias for Wk
                 bias_v: bool = None,  # bias for Wv, if None, use bias for Wv
                 bias_combine: bool = True,  # bias for multi_head_combine
                 sub_glop=False,#only used in glop sub_model while True
                 ):
```

+ **Parameters** of AM
  + env_name: problem to be solved by AM
  + embed_dim: embedding dimension of the network
  + num_heads: number of heads of multi-head attention
  + qkv_dim: dimension of the q, k, v value in attention
  + num_encoder_layers: number of layers of the AM encoder
  + num_decoder_layers: number of layers of the AM decoder
  + normalization: normalization strategy of the model ('batch', 'instance')
  + feedforward_hidden: dimension of the feedforward layer
  + logit_clipping: logit clipping value used in the final compatibility computation
  + use_graph_mean: whether output the mean value of the output of the encoder
  + am_mode: unique parameter for AM method
  + first_placeholder: An indicator to indicate the first node in TSP solution
  + bias: whether exist bias for Wq layer
  + bias_k: whether exist bias for Wk layer
  + bias_v: whether exist bias for Wv layer
  + bias_combine: whether exist bias for final multi head attention combination
  + sub_glop: special parameter for the usage of the attention model in GLOP method

+ Backbone: AM utilize the [Transformer](../developer_doc/methods.md#transformer) to predict next node.



## Training

AM applies autoregressive reinforcement learning [`calss ARREINFORCELightning`]



