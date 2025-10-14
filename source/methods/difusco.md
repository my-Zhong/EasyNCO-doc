# DIFUSCO

DIFUSCO is a graph-based diffusion framework that solves combinatorial optimization problems by formulating them as discrete {0,1}-vector optimization problems and using a denoising diffusion model to generate high-quality solutions.

## DIFUSCOInitialization
**Bases:**  `Initialization`

**Methods**
+ DIFUSCO constructs a heatmap to include the probability of each edge in the optimal solution, and subsequently build a TSP
solution with a greedy manner based on the probability in the heatmap.

## DIFUSCOIteration
**Bases:** `Iteration`

**Methods:**
+ `2-opt`: It improves the greedy solution with the `2-opt` operator under given iterations.
+ `MCTS`: It improves the greedy solution by Monte-Carlo Tree Search (MCTS).

## Usage
You can run the following command lines to execute the code.

```bash
python train.py settings=difusco_settings mode=train problem={problem} settings.model.diffusion_type = {diffusion_type}, settings.diffusion_schedule = {diffusion_schedule}
python train.py settings=difusco_settings mode=train problem=tsp

```
| problem |    diffusion_type     | diffusion_schedule | node_feature_only | parallel_sampling |
|:-------:|:---------------------:|:------------------:|:-----------------:|:-----------------:|
|   tsp   | Categorical (default) |  Linear (default)  |  False (default)  |    1 (default)    |
|   mis   |       Gaussian        |       Linear       |       True        |         1         |


## Tasks

Supported Tasks: TSP, MIS.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`MISGenerator`](../developer_doc/data.md#mis)

## Policy
+ `class DIFUSCOPolicy`
```python
class DIFUSCOPolicy:
    def __init__(self,
                 problem_type,
                 n_layers,
                 hidden_dim,
                 aggregation,
                 diffusion_type,
                 diffusion_schedule,
                 diffusion_steps,
                 sparse_factor,
                 use_activation_checkpoint,
                 parallel_sampling,
                 sequential_sampling,
                 inference_diffusion_steps,
                 inference_schedule,
                 inference_trick,
                 node_feature_only = True)
```
+ **Parameter** of DIFUSCO
  + `aggregation`(`str`): GNN neighborhood [`aggregation`](../developer_doc/methods.md#gnn) scheme.
  + `diffusion_type`(`str`): The type of diffusion technique, default is `Categorical`. Accepted value: `Gaussian`.
  + `diffusion_schedule`(`str`): The schedule used to add noise, accepted values are: `linear`, `cosin`.
  + `diffusion_steps`(`int`): The steps used to add noise.
  + `sparse_factor`(`int`): The degree of sparseness toward the input graph.
  + `parallel_sampling`(`int`): Whether enable parallel computing, to store the heatmap must set to 1.
  + `sequential_sampling`(`int`): Number of solution sequentially generated during denoise step.
  + `inference_diffusion_steps`(`int`): Number of step used to denoise.
  + `Inference_schedule`(`str`): Schedule used to denoise, accepted value: `linear`, `cosine`
  + `Inferenec_trick`(`str`): A trick used to accelerate the denoise of gaussian diffusion.

## Backbone
DIFUSCO predicts the denoised heatmap of optimal edges using Anisotropic Graph Neural Networks ([AGNN](../developer_doc/methods.md#gnn))

## Training

DIFUSCO applies non-autoregressive supervised learning [`class NARSUPERVISELightning`](../developer_doc/phases.md#non-autoregressive-sl) for training