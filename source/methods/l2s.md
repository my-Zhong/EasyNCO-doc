# L2S (Deep Reinforcement Learning Guided Improvement Heuristic for Job Shop Scheduling) 2024 ICLR

L2S proposes a DRL-based improvement heuristic for JSSP. It encodes complete solutions as disjunctive graphs, leverages 
a dual-module GNN (TPM for topology, CAM for job–machine semantics),
and trains a policy via n-step REINFORCE to generate swap operations without full neighborhood evaluation.

## Usage
You can run the following command lines to execute the code.

```bash
python train.py/eval.py settings=l2s_settings mode={train/test} problem=jssp num_job={num_job} num_machine={num_machine} 
decoder_strategy={sampling/greedy} 
```

## Tasks
Supported Tasks: JSSP.

Required Data Generator:
+ [`JSSPGenerator`](../developer_doc/data.md#jssp)


## Training

L2s applies autoregressive reinforcement learning [`class ARREINFORCELightning()`](../developer_doc/phases.md#autoregressive) for training.