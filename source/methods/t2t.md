# T2T

The T2T framework is a method that first uses generative modeling to learn the distribution of 
high-quality solutions for a given problem during training, and then performs a gradient-based search within that solution
space during testing to find an optimal solution for a specific instance.

## T2TInitialization
**Bases:**  `Initialization`

**Methods**
+ T2T constructs a heatmap to include the probability of each edge in the optimal solution, and subsequently build a TSP
solution with a greedy manner based on the probability in the heatmap.

## T2TIteration
**Bases:** `Iteration`

**Methods:**
In T2T Iteration, it first improves the greedy solution with the `2-opt` operator under given iterations. Then, the random noise
is added to the best results, and perform another denoise step to explore better solution.




## Usage
You can run the following command lines to execute the code.

```bash
python train.py settings=t2t_settings mode=train problem={problem} settings.model.diffusion_type={diffusion_type}, settings.diffusion_schedule={diffusion_schedule}, rewrite={rewrite}
python train.py settings=t2t_settings mode=train problem=tsp

```
| problem |    diffusion_type     | diffusion_schedule | node_feature_only | parallel_sampling | Rewrite |
|:-------:|:---------------------:|:------------------:|:-----------------:|:-----------------:|:-------:|
|   tsp   | Categorical (default) |  Linear (default)  |  False (default)  |    1 (default)    |  True   |
|   mis   |       Gaussian        |       Linear       |       True        |         1         |  True   |


## Tasks

Supported Tasks: TSP, MIS.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`MISGenerator`](../developer_doc/data.md#mis)

## Policy
+ `class T2TPolicy`
```python
class T2TPolicy:
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
                 norm,
                 rewrite,
                 rewrite_steps,
                 rewrite_ratio,
                 rewrite_inference_steps,
                 node_feature_only = False)
```
+ **Parameter** of T2T
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
  + `Rewrite`(`bool`): Whether perform gradient-based search based on the initial solution
  + `Rewrite_steps`(`int`): Number of step used for searching
  + `Rewrite_ratio`(`float`): Range of rewrite perform on the initial solution
  + `Rewrite_inference_steps`(`int`): Number of denoise steps used during gradient-based searching

## Model
T2T predicts the denoised heatmap of optimal edges using Anisotropic Graph Neural Networks ([AGNN](../developer_doc/methods.md#gnn))



## Training

T2T applies non-autoregressive supervised learning [`class NARSUPERVISELightning`](../developer_doc/phases.md#non-autoregressive-sl) for training