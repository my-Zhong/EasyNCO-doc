# Pipeline

## `Initialization`

The `Initialization` module provides a framework for initialization solutions within the Pipeline. It serves as an abstract base class, and initialization procedures for various methods should inherit from it.

**Base:**`ABC`

**Parameters:**
+ `policy`(`nn.Module`): Policy network for generating initial solutions.

**Methods**
+ `run`(): Abstract method implemented by subclasses. The environment, data, and policy network interact within this method to generate the initial solution.

```python
def run(self,
        env: EnvBase,
        batch: int,
        strategy: str,
        phase: Literal["train", "eval"],
        **kwargs,
) -> Tuple[TensorDict, Any]
```

### Usage

Each method implements its own Initialization by inheriting from the abstract base class.
For example, `ARInitialization` can implement the initialization process for constructing solutions via autoregressive reinforcement (AR) learning by inheriting from `Initialization`. It overrides the `run` method and introduces `play_episode` to perform an episode play using the provided policy and decoder strategy.

<details> 
    <summary>Expand to view code of class ARInitialization</summary>
    <pre><code>
class ARInitialization(Initialization):
    def run(self,
            env: EnvBase,
            batch: int,
            strategy: str,
            phase: Literal["train", "eval"],
            **kwargs,
    ) -> Tuple[TensorDict, Any]:
        if phase == "train":
            self.policy.train()
            env.load_problems(batch, batch.size(0))
            state_td, policy_out = self.play_episode(env, strategy)
            out = policy_out
        else:
            self.policy.eval()
            with torch.inference_mode():
                env.load_problems(batch, batch_size=batch.size(0))
                state_td, policy_out = self.play_episode(env, strategy)
            aug_reward = policy_out["reward"].reshape(
                env.aug_factor, batch.size(0), env.pomo_size
            )
            # shape: (augmentation, batch, pomo)
            max_pomo_reward, _ = aug_reward.max(dim=2)  # best result from pomo
            # shape: (augmentation, batch)
            no_aug_reward = (
                -max_pomo_reward[0, :].float().mean()
            )  # negative sign to make positive value
            max_aug_pomo_reward, _ = max_pomo_reward.max(dim=0)  # get best result from augmentation
            # shape (batch, )
            aug_score = (
                -max_aug_pomo_reward.float().mean()
            )  # negative sign to make positive value
            out = {
                "no_aug_score": no_aug_reward,
                "aug_score": aug_score,
            }
        return state_td, out
    def play_episode(
        self,
        env,
        decoder_strategy: str = "sampling",
    ) -> Tuple[TensorDict, dict]:
        reset_td = env.reset()
        self.policy.set_decoder_strategy(decoder_strategy)
        self.policy.pre_forward(reset_td)
        likelihood = torch.zeros(size=(env.batch_size[0], env.pomo_size, 0))
        done = False
        reward = None
        state_td = env.pre_step()
        while not done:
            next_td = self.policy(state_td)
            prob = next_td["prob"]
            state_td = env.step(next_td)
            likelihood = torch.cat((likelihood, prob[:, :, None]), dim=2)
            reward = state_td["reward"]
            done = state_td["done"].all()
        policy_out = {
            "reward": reward,
            "likelihood": likelihood,
        }
        return state_td, policy_out
</code></pre> 
</details>


## `Iteration`

The `Iteration` module provides a framework for the iteration process within the pipeline.
The class `Iteration` serves as the base class for all iteration procedures, defining the basic interfaces and structure of the iterative process.
If no iteration is required, the class `NoIteration` can be used to skip this step.

**Base:**`ABC`

**Parameters:**
+ policy (nn.Module): Policy network used to process the initial solution and generate iterative solutions.

**Methods:**
+ `run`(): Abstract method implemented by subclasses. It improves the initial solution by generating enhanced solutions through an iteration-based policy network.

```python
    def run(self,
            td: TensorDict,
            env: EnvBase,
            initialization_out: dict,
            phase: Literal["train", "eval"],
            max_steps: int = 0,
            **kwargs,
    ) -> dict
```

### Usage

Methods that require iteration must implement their own iteration logic by inheriting from the abstract base class `Iteration`.

The class `NoIteration` is a concrete implementation of `Iteration`. It is used when no additional iterative steps are needed. This class simply returns the output from the initialization stage without making any modifications.

<details> 
    <summary>Expand to view code of class NoIteration</summary>
    <pre><code>
    class NoIteration(Iteration):
        def run(self,
                td: TensorDict,
                env: EnvBase,
                initialization_out: dict,
                phase: Literal["train", "eval"],
                max_steps: int = 0,
                **kwargs,
        ) -> dict:
            return initialization_out
</code></pre> 
</details>