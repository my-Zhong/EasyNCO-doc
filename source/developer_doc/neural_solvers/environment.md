# Environments

`Env` is a module for state-action interaction. Its main functions are to **maintain the problem state**, **process model actions**, **compute rewards and state transitions**, and **provide this information to the policy model in a structured form**.

## Base Env

Different problems are implemented with their own environment classes, but all of them include the following main methods to **support the workflow of loading problem data**, **executing actions within the environment**, **transitioning to new states**, and **providing feedback**.

**Bases:** `EnvBase`

**Methods:**
+ `load_problems`: Loads the input problem data to be solved, and performs preprocessing and augmentation based on the configuration.
+ `_reset`: Resets the environment state and initializes the solution state for a new episode.
+ `pre_step`: Provides environment information under the current state to support the model's next decision.
+ `_step`: Receives the model’s action, performs a step update in the environment, updates the state, checks for termination, and computes the reward.

💡For details on various problems and methods, please refer to their respective descriptions.
+ Routing Problems: TSPEnv, ATSPEnv, PCTSPEnv, MTSPEnv, MDVRPEnv, MPDPEnv, OPEnv, SOPEnv
+ Scheduling Problems: FFSPEnv, RCPSPEnv, SMTWTPEnv
+ Packing Problems: KPEnv, MKPEnv, BPPEnv

## DUMMYEnv

Specifically, some problems do not require an environment class. For the sake of framework consistency, we designed a `DUMMYEnv` class, which does not perform any operations.

<details> 
    <summary>Expand to view code of class DUMMYEnv</summary>
    <pre><code>
class DUMMYEnv(EnvBase):
    # batch_locked = False
    def __init__(self,
                 problem_size: int,
                 pomo_size: int = 1,  # multi_start trajectory, in AM-based models, it is 1 by default
                 device: str = 'cpu',
                 seed: int = 2024,
                 **kwargs):
        super().__init__(device=device)
        self.env_name = kwargs.get('env_name')
        self.problem_size = problem_size
        self.pomo_size = pomo_size
        self.device = device
        self.env_name="dummy"
        self.problems = None
        self.selected_count = None
        self.current_node = None
        self.ninf_mask = None
        self.selected_node_list = None
        #self.first_node = None
        self.dummy_flag_bool = None
        self.dummy_flag_long = None
        self.seed_value = seed
        self._set_seed(seed=seed)
        self.method_name = kwargs.get('method_name')
        self.aug_type = kwargs.get('aug_type', None)
        self.aug_factor = kwargs.get('aug_factor',1)
        self.mix_prop = kwargs.get('mix_prop')
        self.aug_flag = self.aug_type is not None and self.aug_factor > 1
        self.distribution=kwargs.get("distribution","uniform")
    def _set_seed(self, seed: Optional[int]):
        rng = torch.manual_seed(seed)
        self.rng = rng
    def _reset(self,td: TensorDict,batch_size=None) -> TensorDict:
        pass
    def _step(self, td: TensorDict) -> TensorDict:
        pass
    def __getstate__(self):
        """Return the state of the environment. By default, we want to avoid pickling
        the random number generator directly as it is not allowed by `deepcopy`
        """
        pass
    def __setstate__(self, state):
        """Set the state of the environment. By default, we want to avoid pickling
        the random number generator directly as it is not allowed by `deepcopy`
        """
        pass
</code></pre> 
</details>








