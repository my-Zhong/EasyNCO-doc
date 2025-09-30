# Setting

Our platform uses two levels of YAML configuration to keep **global options** and **solver-specific parameters** cleanly separated. All configuration files are compatible with **Hydra**, so you can override any field from the command line if needed.

## 1. **Global Configuration** (`config.yaml`)

This file defines general experiment settings that apply to all solvers:

### Model and Task Configuration
+ **mode**: `train` or `test`
+ **model**: model/method to use
+ **problem**: problem type (e.g., `tsp`, `jssp`).
+ **scale**: problem scale, e.g. TSP node count
+ **num_job**: jobs for JSSP/FJSP
+ **num_machine**: machines for JSSP/FJSP
+ **decoder_strategy**: decoding strategy (`sampling` for training, `greedy` for testing)
+ **cuda**: CUDA device IDs for training/testing
+ **max_epochs**: maximum number of training epochs

### Data and Batch Settings
+ **batch_size**: Batch size for training.
+ **episodes**: Number of training episodes per epoch.
+ **val_episode**: Number of validation episodes.
+ **val_data_path**: Path to the validation dataset.
+ **test_data_path**: Path to the test dataset.
+ **train_data_path**: Path to the training dataset (null means data is dynamically generated).
+ **varying_data_params**: Parameters for dynamic data generation, including scale ranges, distribution types, and capacity ranges

### Environment and Training Settings
+ **seed**: Random seed for reproducibility.
+ **disable_gpu**: If True, forces training on CPU.
+ **strategy**: Strategy for distributed training.
+ **matmul_precision**: Precision setting for matrix multiplications.
+ **precision**: Training precision (e.g., 32-true, 16-mixed).
+ **enable_progress_bar**: Whether to display a progress bar during training.
+ **log_every_n_steps**: Logging frequency (in steps).

### Model Checkpointing
+ **ckpt_path**: Path/URL to resume training from a checkpoint.
+ **save_top_k**: Number of checkpoints to save (-1 = save all, 1 = save only the best).
+ **every_n_epochs**: Save a checkpoint every n epochs.

### Output Directory and Logging
+ **dir**: Experiment result directory, automatically includes the mode, model, problem type, scale, and timestamp for easy experiment tracking.
+ **logger**: Uses TensorBoardLogger for logging.

Tip: You can override any of these values from the command line, e.g.
```bash
python train.py max_epochs=10 batch_size=32
```

## 2. Solver-Specific Configuration (`settings/*.yaml`)

Each solver has its own YAML file inside the settings directory.
These files refine or extend the global settings for a particular algorithm.
The example below illustrates typical sections:

### `env` — Problem Environment

The environment defines the optimization problem to solve and how it will be presented to the solver.
+ Points to the environment class (e.g., TSPEnv, CVRPEnv).
+ Sets problem size (e.g., number of nodes, jobs, or machines).
+ Defines augmentation strategies to increase training diversity.
+ Ensures consistency between solver and dataset.

### `model` — Policy Network

The model block configures the solver’s policy network, i.e. the neural architecture that decides actions.
+ Specifies the implementation (e.g., transformer-style policy, pointer network).
+ Defines core architecture settings (e.g., number of layers, normalization type).
+ Links model capacity (hidden size, attention heads) to problem scale.

### `initialization` — Module Setup

The initialization module defines how training or evaluation starts.

### `iteration` — Module Setup

The iteration module defines the step-by-step process of solving the problem.

### `module` — Training Strategy
+ The module encapsulates the solver’s learning logic, often built around reinforcement learning.
+ Defines the training framework (e.g., REINFORCE).
+ Configures optimizers and learning rate schedulers.
+ Specifies decoding strategies (sampling for exploration, greedy for evaluation).
+ Includes limits on episodes, batches, and rollout steps.

### `model_checkpoint` — Saving and Resuming

The checkpoint handler ensures experiments can be paused, resumed, or evaluated later.
+ Defines storage location and file name templates.
+ Controls how often checkpoints are saved.
+ Allows saving only the best models or all of them.

### `trainer` — Execution Control

The trainer relies on PyTorch Lightning and manages the high-level training loop.
+ Sets maximum epochs.
+ Controls logging frequency and progress bar visibility.
+ Configures hardware options (precision, GPU usage).
+ Applies safety features such as gradient clipping.

### `test_loader` — Evaluation Setup

The test loader provides access to pretrained models for benchmarking.
+ Defines where checkpoints are stored.
+ Points to the correct filename for loading a model.
+ Ensures consistent evaluation between solvers.
