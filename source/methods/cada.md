# CaDA (Cross-domain generalized Attentional DRL)

CaDA is a multi-task neural solver for Vehicle Routing Problems (VRPs) that learns a generalized policy to solve various VRP variants simultaneously.

## Initialization

**Bases:** `Initialization`

**Methods:** `CaDAInitialization`

The inputs consist of a tensor containing coordinates, demands, time windows, and other constraints for the Multi-Task VRP (MTVRP) instance. After initialization, CaDA constructs a routing solution step by step using a Prompt-conditioned Transformer architecture. 

### Policy
+ `class CaDAPolicy`
```python
class CaDAPolicy(nn.Module):
    def __init__(
        self,
        embedding_dim: int = 128,
        encoder_layer_num: int = 6,
        qkv_dim: int = 16,
        head_num: int = 8,
        ff_hidden_dim: int = 512,
        logit_clipping: int = 10,
        norm_type: str = 'rms',
        use_sparse: str = 'topk',
        **kwargs
    ):
```

## Usage

You can evaluate the CaDA model on both 50-node and 100-node problem scales. Ensure you use the proper settings for each scale to avoid memory issues.

### Scale 50

For instances with 50 nodes, run the following command:

```bash
python eval.py \
    settings=cada50_settings \
    model=cada \
    problem=mtvrp \
    scale=50 \
    cuda=[0] \
    test_data_path=cada_data/50_*.pt \
    settings.test_loader.model_dirpath=pretrained/cada50_2024-1111-1139 \
    settings.test_loader.model_filename=checkpoint-300.pt \
    settings.module.decoder_strategy=greedy \
    settings.module.val_aug_flag=True \
    settings.env.aug_factor=8 \
    episodes=10000 \
    batch_size=200
```
> **Notes for Scale 50**: 
> - Default `batch_size` is `200`.

### Scale 100

For instances with 100 nodes, the problem scale is computationally heavier.

```bash
python eval.py \
    settings=cada100_settings \
    model=cada \
    problem=mtvrp \
    scale=100 \
    cuda=[0] \
    test_data_path=cada_data/100_*.pt \
    settings.test_loader.model_dirpath=pretrained/cada100_2024-1121-1355 \
    settings.test_loader.model_filename=checkpoint-300.pt \
    settings.module.decoder_strategy=greedy \
    settings.module.val_aug_flag=True \
    settings.env.aug_factor=8 \
    episodes=10000 \
    batch_size=100
```

> **Notes for Scale 100**:
> - Highly recommended to decrease `batch_size` to `100` (or lower) to prevent Out-Of-Memory (OOM) errors during the 8x Geometric Augmentation evaluation.

## Task

Supported Tasks: 16 MTVRP variants including `cvrp`, `ovrp`, `vrpb`, `vrpbl`, `vrpbtw`, `ovrpb`, `ovrpbltw`, `ovrpl`, `vrptw`, `vrpl`, `ovrptw`, `vrpltw`, `ovrpbtw`, `ovrpltw`, `vrpbltw`, `ovrpbl`.

Required Data Generator:
+ [`MTVRPGenerator`](../developer_doc/data.md#mtvrp)
