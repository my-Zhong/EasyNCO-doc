# Platform structure

## Pipeline
Our platform consists of five core parts, which interact to establish a complete solution generation pipeline. They are as follows:
1. **Base**: Manages solver invocation, controlling both training and evaluation procedures.
2. **Data**: Provides built-in instance generators and readers for various problems, all generated or read instances are subsequently converted into a unified "DataLoader" format for direct consumption by the solvers.
3. **Solvers**: Contains specific implementations of various solvers. For neural solvers, the solving process is decomposed into four key components: a backbone network, an environment, a unified pipeline (including a required initialization stage and optional iteration stages), and solver-specific policies.
4. **Settings**: Manages parameter configurations through solver-specific YAML files based on Hydra library.
5. **Utilities**: Offers a collection of utility classes and functions to support the operations of other components.


## Project structure


```bash
├─ data            # Datasets and generation scripts
├─ exact_solver    # Classical exact solvers
├─ neural_solver   # Learning–based neural solvers
│   ├─ backbones   # Model backbones (e.g., GNN, Transformer)
│   ├─ envs        # Training and evaluation environments
│   ├─ methods     # Neural algorithms and policies
│   ├─ pipeline    # Full workflow: initialization and iteration
│   └─ utils       # Helper functions and tools related to neural_solver
├─ phase           # Experiment phases for neural solvers
├─ pretrained      # Pretrained model checkpoints and weights
├─ setting         # Global configuration files and parameter templates
└─ utils           # Helper functions and tools for logging, visualization, ...
```