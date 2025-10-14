# OMNI
Omni focuses on the generalization challenge of neural methods in Vehicle Routing Problems (VRPs) and proposes a universal meta-learning framework to achieve "full generalization" across problem scales and distributions.

## Usage
You can run the following command lines to execute the code.

Omni train in diverse scale [50,200]

```bash
python train.py settings=omni_settings mode=train problem={tsp/cvrp}
python eval.py settings=omni_settings mode=eval problem={tsp/cvrp} scale={scale}
```

### Tasks

Supported Tasks: TSP, CVRP

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)
+ [`CVRPGenerator`](../developer_doc/data.md#cvrp)


## Training

Omni applies autoregressive reinforcement learning [`class ARREINFORCELightning()`](../developer_doc/phases.md#autoregressive) for training.