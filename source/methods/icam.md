# ICAM (Instance-Conditioned Adaptation for Large-scale Generalization  of Neural Combinatorial Optimization)

ICAM is a reinforcement learning-based neural combinatorial optimization model that utilizes instance-conditioned adaptation and a multi-stage training scheme to achieve efficient large-scale generalization for routing problems such as TSP and CVRP.

## Usage
You can run the following command lines to execute the code.

```bash
python train.py/eval.py settings=udc_settings mode={train/test} problem={tsp/cvrp} scale={problem size} decoder_strategy={sampling/greedy}
```

## Tasks

Supported Tasks: TSP, CVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)