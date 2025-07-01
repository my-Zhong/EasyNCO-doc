# LIH
LIH is a combinatorial optimization framework that combines deep reinforcement learning with a self-attention-based architecture, leveraging learned improvement heuristics and policy network-guided local search to derive high-quality solutions for routing problems. 


## Usage
You can run the following command lines to execute the code.

```bash
python train.py settings=lih_settings mode=train problem={problem} settings.module.T={T} settings.module.n_step_return={n_step_return}
python train.py settings=lih_settings mode=train problem=tsp
python train.py settings=lih_settings mode=train problem=tsp settings.module.T=200 settings.module.n_step_return=4
```


| problem | scale |  T  | n_step_return |
|:-----:|:-----:|:---:|:-------------:|
| tsp |  20   | 200 |       4       |
|     |  50   | 200 |       4       |
|     |  100  | 200 |       4       |
| cvrp |  20   | 360 |      10       |
|      |  50   | 480 |      12       |
|      |  100  | 480 |      12       |


### Tasks

Supported Tasks: TSP, CVRP

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)

## Policy
**main loop**

+ LIH starts by randomly generating an initial solution. The neural network then selects a pair of nodes for the 2-opt operation to improve the solution iteratively.

+ In the CVRP problem, each node is represented by a 7-dimensional vector, formed by concatenating the previous node $x_{t-1}$, the current node $x_t$, the next node $x_{t+1}$, and the demand.




## Training

LIH applies autoregressive reinforcement learning [`class ARREINFORCELightning()`](../developer_doc/phases.md#autoregressive) for training.

**training paradigm**

+ TSP is trained using the A2C algorithm.  
+ CVRP is trained using the PPO algorithm
+ Both of the training processes apply multi-step discounted rewards to improve stability and learning efficiency.