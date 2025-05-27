# DeepACO

DeepACO is a general combinatorial optimization framework that combines graph neural networks with ant colony optimization, using learned heuristics and neural-guided local search to find high-quality solutions.

## Usage
You can run the following command lines to execute the code.

```bash
python train.py settings=deepaco_settings mode=train problem=tsp
python train.py settings=deepaco_settings mode=train problem=cvrp
python train.py settings=deepaco_settings mode=train problem=op settings.model.feats=2 settings.model.phe_net=True
python train.py settings=deepaco_settings mode=train problem=pctsp settings.model.feats=2 settings.model.phe_net=True
python train.py settings=deepaco_settings mode=train problem=sop settings.model.phe_net=True settings.model.update_x=False
python train.py settings=deepaco_settings mode=train problem=smtwtp settings.model.feats=2 settings.model.phe_net=True settings.model.update_x=False
python train.py settings=deepaco_settings mode=train problem=rcpsp settings.model.feats=5 settings.model.edge_feats=2
python train.py settings=deepaco_settings mode=train problem=mkp settings.model.feats=5 settings.model.phe_net=True
python train.py settings=deepaco_settings mode=train problem=bpp
```



## Data

Supported Tasks: TSP, CVRP, OP, PCTSP, SOP, SMTWTP, RCPSP, MKP, BPP.

Required Data Generator:
+ [`TSPGenerator`](../../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../../developer_doc/data.md#cvrp)
+ [`OPGenerator`](../../developer_doc/data.md#op)
+ [`PCTSPGenerator`](../../developer_doc/data.md#pctsp)
+ [`SOPGenerator`](../../developer_doc/data.md#sop)
+ [`SMTWTPGenerator`](../../developer_doc/data.md#smtwtp)
+ [`RCPSPGenerator`](../../developer_doc/data.md#rcpsp)
+ [`MKPGenerator`](../../developer_doc/data.md#mkp)
+ [`BPPGenerator`](../../developer_doc/data.md#bpp)

## Backbone

DeepACO predicts heuristic information using [GNN](../../developer_doc/methods.md#gnn). In particular, [Transformer](../../developer_doc/methods.md#transformer) has also been used in MKP.

+ GNN
  + `EmbNet()`: update node and edge features to obtain edge embeddings.
  + `ParNet()`: inherit from MLP to predict heuristic information.

## Policy

+ `def load_policy()`: load the corresponding ACO algorithm based on different problems.
+ *ACO* algorithms
  + `class ACO_TSP()`
  + `class ACO_CVRP()`
  + `class ACO_OP()`
  + `class ACO_PCTSP()`
  + `class ACO_SOP()`
  + `class ACO_SMTWTP()`
  + `class ACO_RCPSP()`
  + `class ACO_MKP()`
  + `class ACO_BPP()`
+ **Parameters** of ACO
  + `n_ants`(`int`): The number of ants used to construct solutions in each iteration.
  + `decay`(`float`): The evaporation rate of pheromones in each iteration.
  + `alpha`(`int`): Determines how much ants rely on pheromone concentration.
  + `beta`(`int`): Determines how much ants rely on heuristic information.
  + `elitist`(`bool`): Indicates whether to use the elitist strategy, i.e., whether the best path receives additional pheromone reinforcement.
  + `min_max`(`bool`): Indicates whether to use the Min-Max ACO strategy, which imposes upper and lower bounds on pheromone concentration.
  + `min`(`float`): Sets the minimum allowable pheromone value in the Min-Max ACO strategy.


## Training

DeepACO applies non-autoregressive reinforcement learning [`class NARREINFORCENARLightning()`](../../developer_doc/phases.md#autoregressive) for training.