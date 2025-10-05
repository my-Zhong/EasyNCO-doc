# NLNS

Neural Large Neighborhood Search (NLNS) is based on the Large Neighborhood Search (LNS) framework, where solutions are improved by repeatedly destroying and repairing parts of them. The repair step, which is more complex, is handled by a deep neural network trained with policy gradient reinforcement learning.

## `NLNSInitialization`

**Bases:** `Initialization`

**Parameters:**

+ `env`: `torchrl.envs.EnvBase`: the environment containing the problems to be solved.
+ `batch`: Batch size, used to load multiple VRP instances.
+ `strategy`: A string indicating the strategy (used for labeling purposes, not affecting the greedy initialization).
+ `phase`: "train" or "eval", specifies whether the run is in training or evaluation mode.

**Methods:**
+ 

### ``

A helper class that generates initial solutions using a **greedy** heuristic.

## `NLNSIteration`

**Bases:** `Iteration`

**Parameters:**


## `NLNSPolicy`





## Model


## Usage

NLNS supports two search modes:
+ *batch search*, in which a batch of instances is solved simultaneously
+ *single instance search*, in which a single instance is solved using a parallel search.

You can run the following command lines to execute the code.

```bash
python train.py
```


## Tasks

Supported Tasks: CVRP, SDVRP

Required Data Generator:
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)


## Policy

**Main loop**

+ Start solution generation: The initial solution is generated using a greedy nearest-neighbor heuristic, which builds routes by sequentially adding the nearest customer to each tour starting from the depot. (see `initialization.py`)


+ Destroy operators: Two destroy procedures are used to determine the number of elements to remove from a solution and the method by which they are selected. (see `destroy.py`)
  + *Point-based destroy* removes the customers closest to a randomly selected point from all tours of a solution.
  + *Tour-based destroy* removes all tours closest to a randomly selected point from a solution.

+ Learning to repair solutions: The repair procedure is framed as a reinforcement learning task where a *model* incrementally connects incomplete tours to build complete solutions.
  + NLNS uses Pointer Network to select the nodes for repairing the destroyed partial solution.


## Training

NLNS applies autoregressive reinforcement learning `class ARREINFORCENARLightning()` for training.