# DRHG

DRHG introduces a novel iterative framework that achieves state-of-the-art performance on large-scale routing problems (up to 10,000 nodes) by leveraging a condensed hyper-graph formulation. By reducing undestroyed path segments into fixed hyper-edges, the method significantly lowers computational complexity and enables efficient large-neighborhood search via supervised learning, demonstrating strong generalization on benchmarks like TSPLib and CVRPLib.


## CITATION 
(should be deleted later)
@article{Li_Liu_Wang_Zhang_2025,   
title={Destroy and Repair Using Hyper-Graphs for Routing},  
volume={39},  
number={17}, 
journal={Proceedings of the AAAI Conference on Artificial Intelligence}, 
author={Li, Ke and Liu, Fei and Wang, Zhenkun and Zhang, Qingfu}, 
year={2025}, 
pages={18341-18349}}


## `DRHGInitialization`

**Bases:** `Initialization`

**Methods:**

+ In TSP problem, the DRHG policy uses *random insertion* to generation initial solution, which you can refer to `insertion` method.
+ In CVRP problem, the DRHG policy uses *swap* in `utils.py` to generation initial solution in a fast speed.


## `DRHGIteration`

**Bases:** `Iteration`

**Methods:**

+ **main Parameters** of `Iteration`
    + *Iter_budget*: The total iteration budget for improving solution.
    + *destroy_mode*: There are two options for *destroy_mode*:`knn_location` and `fixed_size`, `knn_location` as default method in evaluation.
    + *center_type*: `equally`: Select the center point according to a certain regularity. `random`: choose the center point sampled from uniform distribution.
    + *coordinate_transform*: scale the node coordinates in [0,1]. make sure that the x-span is larger than y-span.


## Model

There are two components in DRHG model architecture: the `Encoder` and the `Decoder.`



| Components | type |  function  |
|:-----:|:-----:|:---:|
|   Encoder   |  MLP   | output node embeddings  |
|   Decoder   |  MLP   | ouput node selection probability |


```python
class DRHGEncoder(nn.Module):
    def __init__(
        self,
        problem_type: str = 'tsp',
        node_dim: int = 2,
        embed_dim: int = 128,
        num_heads: int = 8,
        qkv_dim: int = 16,
        feedforward_hidden: int = 512,
        normalization: str = None,
        bias: bool = False,
    ):
```
```python
class DRHGDecoder(nn.Module):
    def __init__(
        self,
        problem_type: str = 'tsp',
        num_layers: int = 6,
        embed_dim: int = 128,
        num_heads: int = 8,
        qkv_dim: int = 16,
        feedforward_hidden: int = 512,
        normalization: str = None,
        bias: bool = False,
        first_placeholder: bool = False,
    ):
```




## Policy

+ *DRHG_policy*
    + *Backbone*
    + *encoder*: the encoder contains one MLP layer, mapping all node attributes to high-dimension vector.
    + *decoder*: similar to `LEHD` decoder, it contains six attention layer, connecting the isolation point step by step. 

+ **main Parameters** of DRHG
    + *destruction_mask*: mask a group of nodes from complete solution, and re-connect later.
    + *knn_k*: the number of nodes in destruction group.
    + *current_best_solution*: record the current best solution. If current solution is improved, update the `best solution`. 

+ *utils*
    + *assemble_solution*: the rules for intergating sub-solution to previous solution and generating new solution.



## Usage
You can run the following command lines to execute the code.

```bash
python eval.py settings=drhg_settings mode=test model=drhg scale={scale} batch_size={batch_size} episodes={episodes} problem={problem}
```


| problem | pretrained |  k_min  | k_max | center_type |
|:-----:|:-----:|:---:|:-------------:|:-----------:|
|  tsp |  100   | 20  |       100       |    equally    |
| cvrp  |  100   | 20  |       100       |    random    |




## Tasks

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)



