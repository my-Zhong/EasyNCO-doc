# MatNet (Matrix Encoding Networks for Neural Combinatorial Optimization) 2021 nips

MatNet is a neural matrix encoding network that processes bipartite graph-structured data with a dual attention mechanism to solve asymmetric combinatorial optimization problems like ATSP and FFSP through end-to-end reinforcement learning.

## Usage
You can run the following command lines to execute the code.

```bash
python train.py/eval.py settings=matnet_settings mode={train/test} problem={atsp/ffsp} scale={problem size} decoder_strategy={sampling/greedy} settings.model.num_encoder_layers={5/3} settings.env.pomo_size=24
```

## Tasks

Supported Tasks: ATSP, FFSP.

Required Data Generator:
+ [`ATSPGenerator`](../developer_doc/data.md#atsp)
+ [`FFSPGenerator`](../developer_doc/data.md#ffsp)