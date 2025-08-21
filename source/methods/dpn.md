# DPN (DPN: Decoupling Partition and Navigation for Neural Solvers  of Min-max Vehicle Routing Problems) 2024 ICML

DPN is a neural solver for min-max vehicle routing problems that decouples partition and navigation tasks using a specialized attention-based encoder and leverages agent-permutation symmetry to enhance solution quality.

## Usage
You can run the following command lines to execute the code.

```bash
python train.py/eval.py settings=dpn_settings mode={train/test} problem={mtsp/mpdp/mdvrp/fmdvrp} scale={problem size} decoder_strategy={sampling/greedy} settings.env.agent_min={agent_min} settings.env.agent_max={agent_max} settings.env.depot_min={depot_min} settings.env.depot_max={depot_max}
```

**Note:** For the setting in mTSP and mPDP, the `scale` parameter includes the single depot. For MDVRP and FMDVRP, the `scale` includes all depots.

For more details, the training settings are listed in the tables below. (original paper table5)
| Problem | mPDP50 & mTSP50 | mPDP100 & mTSP100 |
| :--- | :--- | :--- |
| **Fintune or not** | No | No |
| **number of agents($M$)** | [2,10] | [2,10] |
| **number of depots($D$)** | 1 | 1 |
| **number of encoder layers (L)** | 6 | 6 |
| **learning rate** | 1.00E-04 | 1.00E-04 |
| **learning rate decay**| 1 | 1 |
| **batch size** | 256 | 256 |
| **epoch size** | 500 | 500 |
| **epochs** | 256000 | 256000 |
| **number of permutations ($K$)** | 60 | 60 |

| Problem | mPDP200 & mTSP200 | mPDP500 & mTSP500 |
| :--- | :--- | :--- |
| **Fintune or not** | From 100 | From 100 |
| **number of agents($M$)** | [10,20] | [30,50] |
| **number of depots($D$)** | 1 | 1 |
| **number of encoder layers (L)** | 6 | 6 |
| **learning rate** | 1.00E-05 | 1.00E-05 |
| **learning rate decay**| 1 | 1 |
| **batch size** | 64 | 16 |
| **epoch size** | 20 | 20 |
| **epochs** | 64000 | 16000 |
| **number of permutations ($K$)** | 60 | 60 |

| Problem | MDVRP50 & FMDVRP50 | MDVRP100 & FMDVRP100 |
| :--- | :--- | :--- |
| **Fintune or not** | No | No |
| **number of agents($M$)** | [2,10] | [2,10] |
| **number of depots($D$)** | [2,10] | [2,10] |
| **number of encoder layers (L)** | 6 | 6 |
| **learning rate** | 1.00E-04 | 1.00E-04 |
| **learning rate decay**| 1 | 1 |
| **batch size** | 256 | 256 |
| **epoch size** | 500 | 500 |
| **epochs** | 256000 | 256000 |
| **number of permutations ($K$)** | 60 | 60 |

| Problem | MDVRP50-F & FMDVRP50-F | MDVRP100-F & FMDVRP100-F |
| :--- | :--- | :--- |
| **Fintune or not** | From 50 | From 100 |
| **number of agents($M$)** | [3,7] | [5,10] |
| **number of depots($D$)** | 8 | 8 |
| **number of encoder layers (L)** | 6 | 6 |
| **learning rate** | 1.00E-05 | 1.00E-05 |
| **learning rate decay**| 1 | 1 |
| **batch size** | 128 | 128 |
| **epoch size** | 20 | 20 |
| **epochs** | 128000 | 128000 |
| **number of permutations ($K$)** | 60 | 60 |

## Tasks

Supported Tasks: min-max mTSP, min-max mPDP, MDVRP, FMDVRP.

Required Data Generator:
+ [`TSPGenerator`](../developer_doc/data.md#tsp)

## Training

DPN applies autoregressive reinforcement learning [`class ARREINFORCENARLightning()`](../developer_doc/phases.md#autoregressive) for training.