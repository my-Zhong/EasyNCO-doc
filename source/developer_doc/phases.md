# Phases

This provides an overview of the `ARREINFORCELightning` class, a PyTorch Lightning module designed for NCO. The implementation integrates **initialization**, **iteration**, and **baseline** strategies, and supports flexible training and evaluation across different environments. It enables consistent experimentation across multiple algorithms with strong support for logging, validation, and testing workflows.

The class orchestrates the following components:
+ **Policy Network:** Defines the decision-making mechanism.
+ **Environment (EnvBase):** Simulates the optimization problem.
+ **Initialization & Iteration Modules:** Handle problem-specific rollout and optimization strategies.
+ **Baseline:** Supports variance reduction methods for REINFORCE.
+ **Data Loading:** Flexible dataloader configuration for training, validation, and testing.

## `ARREINFORCELightning`

```python
class ARREINFORCELightning(
    policy: nn.Module,
    env: EnvBase,
    problem_size: int,
    initialization: Initialization,
    iteration: Iteration,
    baseline: str='shared',
    optimizer_params: dict=None,
    optimizer_type: str = 'Adam',
    batch_size: int = 128,
    episodes: int = 1280,
    scheduler_type: str = None,
    val_aug_flag: bool = False,
    val_episode: int = 10000,
    val_batch_size: int = 10000,
    bl_data_size: int = 10000,
    bl_batch_size: int = 10000,
    val_env: EnvBase = None,
    decoder_strategy: str = "sampling",
    every_n_steps_output: int = 100,
    **kwargs)
```

**Base**: `lightning.LightningModule`

## Parameters

**Core Components**

+ `policy` (`nn.Module`): The neural policy network used to generate actions.
+ `env` (`EnvBase`): The RL environment defining the optimization problem (e.g., TSP, CVRP).
+ `initialization` (`Initialization`): Handles state initialization and data preprocessing for each episode.
+ `iteration` (`Iteration`): Defines the iterative procedure for policy rollout and optimization.

**Training Parameters**

+ `problem_size` (`int`): Size of the optimization problem (e.g., number of nodes).
+ `batch_size` (`int`): Training batch size.
+ `episodes` (`int`): Number of training episodes.
+ `optimizer_type` (`str`): Optimizer selection (default: `Adam`).
+ `optimizer_params` (`dict`): Optimizer configuration (learning rate, betas, etc.).
+ `scheduler_type` (`str`): Optional learning rate scheduler (`MultiStepLR`, `ExponentialLR`).
+ `every_n_steps_output` (`int`): Frequency for logging training progress.

**Baseline Parameters**

+ `baseline` (`str`): Baseline strategy (`shared`, `critic`, `rollout`, etc.).
+ `bl_data_size` (`int`): Dataset size for baseline evaluation.
+ `bl_batch_size` (`int`): Batch size for baseline updates.

**Validation & Testing**

+ `val_aug_flag` (`bool`): Whether to apply data augmentation during validation.
+ `val_episode` (`int`): Number of validation episodes.
+ `val_batch_size` (`int`): Validation batch size.
+ `val_env` (`EnvBase`): Separate environment for validation.
+ `test_episodes` (`int`): Number of test episodes.
+ `test_batch_size` (`int`): Batch size for test.

**Method-Specific Settings**

+ `method_name` (`str`): Algorithm identifier (e.g., `lih`, `omni`, `udc`).
+ `decoder_strategy` (`str`): Decoding method (`sampling`, `greedy`, etc.).
+ `elg_start_step` (`int`): Starting step for ELG methods.
+ `train_data_path` (`str`): Path to training dataset.
+ `test_data_path` (`str`): Path to test dataset.
+ `val_data_path` (`str`): Path to validation dataset.
+ `customized` (`bool`): Whether initialization and iteration should be composed manually.

**Others**

+ `do_val` (`bool`): Whether validation should be performed.
+ `improve` (`bool`): If `True`, enables manual optimization and custom iteration handling.
+ `time_estimator` (`TimeEstimator`): Utility for tracking training and evaluation time.


## Methods

**Hooks**

+ `on_fit_start()`: Prepares devices and resets timers before training starts.
+ `on_train_start()`: Initializes the baseline with a copy of the environment.
+ `on_test_start()`: Resets environment and timers before testing.
+ `on_train_epoch_end()`: Logs epoch-level metrics, evaluates baseline, and resets meters.
+ `on_validation_epoch_end()`: Logs validation metrics and resets state.
+ `on_test_end()`: Summarizes test results, reports timing and performance statistics.

**Training & Evaluation**

+ `training_step(batch, batch_idx)`: Executes initialization and iteration, computes REINFORCE loss, logs reward/loss, and updates metrics.
+ `test_step(batch, batch_idx, env=None, decoder_strategy=None)`: Evaluates the model on a batch, handles problem-specific cases, logs scores and gaps.
+ `validation_step(batch, batch_idx)`: Runs evaluation with greedy decoding if validation is enabled.

**Loss & Optimization**

+ `calculate_loss(policy_out)`: Computes REINFORCE loss with baseline adjustment, supports method-specific loss functions.
+ `configure_optimizers()`: Instantiates optimizer (`Adam`, `AdamW`) and optional scheduler.

**Data Handling**

+ `train_dataloader()`: Creates data loader for training episodes.
+ `test_dataloader()`: Creates data loader for test dataset.
+ `val_dataloader()`: Creates validation data loader.

**Logging & Utilities**

+ `log()` / `log_dict()`: Integrated with PyTorch Lightning’s logging system.
+ `AverageMeter`: Tracks mean values for score, loss, and gap.
+ `TimeEstimator`: Provides elapsed and remaining time estimation.


## Usage

```python
from EasyNCO.neural_solvers.methods.ar_reinforce import ARREINFORCELightning
from EasyNCO.neural_solvers.pipeline import Initialization, Iteration

model = ARREINFORCELightning(
    policy=my_policy_network,
    env=my_environment,
    problem_size=50,
    initialization=Initialization(...),
    iteration=Iteration(...),
    optimizer_params={"optimizer": {"lr": 1e-4}, "scheduler": {"milestones": [100], "gamma": 0.9}},
    optimizer_type='Adam',
    batch_size=128,
    episodes=1280,
    baseline='shared'
)

trainer = pl.Trainer(max_epochs=100)
trainer.fit(model)
trainer.test(model)
```
