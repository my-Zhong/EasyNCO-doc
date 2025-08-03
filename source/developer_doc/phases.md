# Phases

这里将会介绍神经求解器进行训练、验证、测试阶段的模块`ARREINFORCELightning`。这是一个基于 `PyTorch Lightning` 框架的学习模块，集成了初始化、迭代优化器、baseline 模型，并支持训练、验证、测试等流程。

## `class ARREINFORCELightning`

```python
ARREINFORCELightning(
    policy: nn.Module,
    env: EnvBase,
    baseline: str,
    problem_size: int,
    optimizer_params: dict,
    initialization: Initialization,
    iteration: Iteration,
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
### Parameters

policy	nn.Module	策略网络，用于决策
env	EnvBase	环境实例，定义问题（如 TSP/CVRP）
baseline	str	baseline 类型，如 critic, rollout, warmup 等
problem_size	int	问题规模（例如，TSP 节点数）
optimizer_params	dict	优化器参数配置
initialization	Initialization	初始化模块，用于生成初始解
iteration	Iteration	迭代模块，用于进行训练步骤
batch_size	int	训练时每批次样本数
episodes	int	总训练 episode 数
optimizer_type	str	优化器类型，支持 Adam, AdamW
scheduler_type	str	学习率调度器，如 MultiStepLR
val_aug_flag	bool	是否使用增强数据验证
val_episode	int	验证集 episode 数
val_batch_size	int	验证时 batch size
test_data_path	str	测试数据路径
train_data_path	str	训练数据路径（可为空）
elg_start_step	int	控制 ELG 方法的起始 step
method_name	str	方法名，如 omni, lih, glop 等
do_val	bool	是否进行验证
fine_tune	Namespace	用于微调时的配置对象

### Main Attributes




### Methods

+ `training_step`(`batch`: tensor, `batch_idx`: int): 训练阶段每个 batch 的核心逻辑。包含初始化、迭代、计算损失、更新 baseline、记录日志。

+ `test_step`(`batch`, `batch_idx`, `env`=None, `decoder_strategy`=None): 单个 batch 的测试逻辑。支持使用不同策略（如 greedy, sampling）评估 policy 性能。

+ `validation_step`(`batch`, `batch_idx`): 验证集调用 test_step。控制是否开启验证、使用 val_env 等。

+ `calculate_loss`(`policy_out`): 使用 REINFORCE 算法计算 loss。兼容不同算法（如 omni、lih）可自定义 loss。

+ `configure_optimizers`(): 返回优化器和学习率调度器。

+ `train_dataloader`(): 根据配置加载训练集。支持数据增强、glop/omni 特殊流程。

+ `test_dataloader`(): 加载测试数据集。

+ `val_dataloader`(self): 加载验证数据集（如果启用验证）。

+ `on_fit_start`(): 设置设备、初始化时间估计器等。

+ `on_train_start`(): 初始化 baseline（如 rollout、critic 等）。

+ `on_train_epoch_end`(): 记录 epoch-level 的训练指标，并更新 baseline。

+ `on_validation_epoch_end`(): 记录验证集性能（score, augmented score）。

+ `on_test_start`(): 测试开始时的日志与时间估算。

+ `on_test_end`(): 测试结束时的日志与时间估算。



