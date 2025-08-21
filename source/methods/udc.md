# UDC (UDC: a unified neural divide-and-conquer framework for large-scale combinatorial optimization problems) 2024 nips

UDC is a unified neural divide-and-conquer framework that employs a graph neural network for global partitioning and a fixed-length sub-solver with a novel Divide-Conquer-Reunion training scheme to solve large-scale combinatorial optimization problems efficiently and generally.

## Usage
You can run the following command lines to execute the code.

```bash
python train.py/eval.py settings=udc_settings mode={train/test} problem={tsp/cvrp} scale={problem size} decoder_strategy={sampling/greedy} settings.model.feats={2/3} settings.module.max_steps=n
```

## Tasks

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)