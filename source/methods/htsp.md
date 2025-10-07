# HTSP

H-TSP is an end-to-end deep reinforcement learning approach that can handle TSP problems with up to 10,000 nodes. H-TSP uses a hierarchical reinforcement learning approach, which means it breaks down the big problem into smaller parts and solves them in two steps.


## CITATION 
(should be deleted later)
@inproceedings{pan2023h-tsp,
    author = {Pan, Xuanhao and Jin, Yan and Ding, Yuandong and Feng, Mingxiao and Zhao, Li and Song, Lei and Bian, Jiang},
    title = {H-TSP: Hierarchically Solving the Large-Scale Traveling Salesman Problem},
    booktitle = {AAAI 2023},
    year = {2023},
    month = {February},
    url = {https://www.microsoft.com/en-us/research/publication/h-tsp-hierarchically-solving-the-large-scale-traveling-salesman-problem/},
}



## `HTSPInitialization`

**Bases:** `Initialization`

**Methods:**

+ In training mode, the HTSP policy is executed to collect experiences(*explore_trajectory*, *update_buffer*) and use PPO to update actor and crtic network. 
+ In evaluation mode, the HTSP policy sample a point through upper level network, and use this point and its related points to construct a path through lower level solver, which is realized by *HTSP_test_step* function.


## Model

The upper level network for HTSP, includes `IMPALAEnocder`, `ActorPPO`, `CriticPPO`.

| Network | type |  function  |
|:-----:|:-----:|:---:|
|   IMPALAEncoder   |  CNN   | output a feature map  |
|   ActorPPO    |  MLP  | ouput the coordinate of center point  |
|   CriticPPO    |  MLP  | ouput the value of state  |

```python
class IMPALAEncoder(nn.ModuleDict):
    def __init__(
        self,
        input_dim,
        embedding_dim,
        bev_range=None,
        bev_pixel_size=None,
        depths=None,
    **kwargs):
```
```python
class ActorPPO(nn.Module):
    def __init__(
        self, 
        state_dim, 
        mid_dim, 
        action_dim, 
        init_a_std_log=-0.5, 
        **kwargs):
```python

```python
class CriticPPO(nn.Module):
    def __init__(
        self, 
        state_dim, 
        mid_dim, 
        _action_dim,
        **kwargs):
```

## Policy

+ *HTSP_policy*
    + *Backbone*
        + *encoder*: the TSP graph is converted into a pseudo-image by discretizing the 2D space into a grid.The CNN encoder's function is to process a "pseudo-image" that represents the Traveling Salesman Problem (TSP) instance.
        + *actor*: Its purpose is to select a small subset of important nodes (up to 200) from the overall TSP problem.
        + *critic*: The primary function of the critic is to estimate the "value" of a given state. 

+ **main Parameters** of HTSP
    + *fragment_length*: The length of the sub-fragment.
    + *target_steps*: The maximum steps in a trajectory.
    + *k*: The number of neighbors in knn.

+ *Lower_slover*
    + *POMO*: It employs the POMO strategy[pomo](../methods/pomo.md#pomo) to generate solutions for sub-fragments. In particular, [Transformer](../developer_doc/methods.md#transformer) has also been applied in the solver.
    + *LKH*: A *exact solver* [LKH](../developer_doc/exact_solvers.md#LKH) is used to solve the sub-fragment problem.



## Usage
You can run the following command lines to execute the code.

```bash
python train.py settings=htsp_settings mode=train model=htsp scale={scale} batch_size={batch_size} episodes={episodes} settings.model.low_env.auto_reset=True
python eval.py settings=htsp_settings mode=test model=htsp scale={scale} batch_size={batch_size} episodes={episodes} setttings.model.low_env.auto_reset=False test_data_path={PATH} 
```


| problem | scale |  k  | fragment_length | target_steps |
|:-----:|:-----:|:---:|:-------------:|:-----------:|
|   tsp |  1000   | 40  |       200       |    64    |
|       |  2000   | 40  |       200       |    64    |
|       |  5000   | 40  |       200       |    64    |
|       |  10000  | 40  |       200       |    64    |



## Tasks

Supported Tasks: TSP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)




## Training

DeepACO applies PPO learning [`class PPOLightning()`](../developer_doc/phases.md#autoregressive) for training.