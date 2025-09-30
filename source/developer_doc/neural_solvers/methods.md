# Methods

This part contains complete implementations of various NCO methods. For each method, the implementation generally includes the following key components:

## Policy
The policy networks are responsible for decision-making within each method. They generate action probability distributions or directly output specific actions based on the current state. As the core of the model’s decision process, the policy networks determine the behavior during optimization.

## Encoder & Decoder
These networks handle feature extraction (encoding) from input problem data and solution generation (decoding) based on the encoded information. This module supports diverse model architectures tailored to the characteristics and requirements of different problems.

## Initialization & Iteration
The main loop encompasses the **Initialization** and **Iteration** procedures unique to each method. It represents the critical steps for training and inference. This emphasizes modularity, allowing users to flexibly combine different components for algorithm comparison and custom extension.

For details on various methods, please refer to their respective descriptions.
