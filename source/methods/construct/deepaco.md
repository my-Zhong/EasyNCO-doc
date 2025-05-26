# DeepACO

DeepACO is a general combinatorial optimization framework that combines graph neural networks with ant colony optimization, using learned heuristics and neural-guided local search to find high-quality solutions.


## Parameters



## Data & Environment

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

DeepACO predicts heuristic information using [GNN](../../developer_doc/methods.md#gnn).

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


## Training

DeepACO applies non-autoregressive reinforcement learning [`class NARREINFORCENARLightning()`](../../developer_doc/phases.md#autoregressive) for training.