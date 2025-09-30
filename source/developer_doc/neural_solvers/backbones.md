# Backbones
Our platform integrates three mainstream deep learning backbones to accommodate various NCO solvers.

- [Transformer](#transformer)
- [GNN](#gnn)
- [Diffusion](#diffusion)

## Transformer

This module `class TransformerNet` implements a Transformer architecture based on **Multi-Head Attention (MHA)**. It constructs a Transformer backbone composed of multiple layers of **attention mechanisms** and **feed-forward networks**, and leverages **residual connections** and **normalization** to stabilize the training process, thereby enhancing the capability of global modeling over graph-structured or sequential data.

### `TransformerNet`
```python
class TransformerNet(
    num_layers: int = 6,
    num_heads: int = 8,
    qkv_dim: int = 16,
    embed_dim: int = 128,
    normalization: str = "batch",
    feedforward_hidden: int = 512,
    bias: bool = False,
    bias_k: bool = None,
    bias_v: bool = None,
    bias_combine: bool = True,
    )
```

**Parameters**
+ `num_layers` (`int`): Number of stacked Transformer layers.
+ `num_heads` (`int`): Number of attention heads in each layer.
+ `qkv_dim` (`int`): Dimension of each attention head.
+ `embed_dim` (`int`): Dimension of the input embeddings and outputs.
+ `normalization` (`str`): Type of normalization to use, supports *"batch"*, *"layer"*, or *None*.
+ `feedforward_hidden` (`int`): Dimension of the hidden layer in the feed-forward network.
+ `bias_k`, `bias_v`, `bias_combine` (`bool`): Whether to add bias to K, V and output.
+ `multi_head_combine_used` (bool): Whether to use the output layer multi_head_combine.

**Attributes**
+ `MHA_layers` (`nn.ModuleList`): Multi-layer MultiHeadAttentionLayer modules, each wrapped with a SkipConnection.
+ `norm1` (`nn.ModuleList`): Normalization modules applied after attention in each layer.
+ `FF_layers` (`nn.ModuleList`): Feed-forward network modules, each wrapped in a SkipConnection.
+ `norm2` (`nn.ModuleList`): Normalization modules applied after the feed-forward network in each layer.

**Methods**
+ `forward`: Performs multi-layer Transformer encoding on the input tensor. If an external weights dictionary is provided, it uses the external weights for computation; otherwise, it uses the model’s own parameters. Returns a tensor of shape (batch, graph_size, embed_dim).

### Components
It consists of multiple layers, each composed of `MHA layer + Norm + FFN + Norm`.
```python
for i in range(self.num_layers):
    out = self.MHA_layers[i](out)
    out = self.norm1[i](out)
    out = self.FF_layers[i](out)
    out = self.norm2[i](out)
```
+ **MultiHeadAttentionLayer**: It with `SkipConnection` constitutes the main part of the multi-head attention mechanism, supporting weight loading and optional bias settings.
+ **Normalization**: It supports three types of normalization: **batch**, **instance**, and **None**.
+ **FeedForward**: It consists of two fully connected layers with a non-linear activation function (ReLU or GELU).
+ Another **Normalization**

### `MultiHeadAttentionLayer`
```python
class MultiHeadAttentionLayer(
    embed_dim: int,
    num_heads: int = 8,
    qkv_dim: int = 16,
    bias: bool = False,  # bias for Wq
    bias_k: bool = None, # bias for Wk, if None, use bias for Wk
    bias_v: bool = None, # bias for Wv, if None, use bias for Wv
    bias_combine: bool = True,  # bias for multi_head_combine
    multi_head_combine_used : bool = True,
    ):
```

**Parameters**
+ `embed_dim` (`int`): Dimension of the input embeddings.
+ `num_heads` (`int`): Number of attention heads.
+ `qkv_dim` (`int`): Dimension of the query, key, and value vectors for each head.
+ `bias_k`, `bias_v`, `bias_combine` (`bool`): Whether to add bias to K, V and output.
+ `multi_head_combine_used` (bool): Whether to use the output layer multi_head_combine.

**Attributes**
+ `Wq`: Maps input embeddings (q_input) to query vectors for multiple attention heads.
+ `Wk`: Maps input embeddings (kv_input) to key vectors for multiple attention heads.
+ `Wv`: Maps input embeddings (kv_input) to value vectors for multiple attention heads.
+ `multi_head_combine`: Merges the concatenated outputs of all heads back into the original embedding dimension.

**Methods**
+ `forward`: Performs the full multi-head attention computation, including linear projections for queries, keys, and values, head splitting, attention scoring and weighting, and the final multi-head output combination

### Other utils
+ `class Compatibility`: Computes the compatibility score between the current query state and the remaining candidate nodes, producing a probability distribution for action selection. It is used in the RL-based NCO models, such as Attention Model and POMO Model.
+ `def reshape_by_heads`: It is used to reshape the linearly projected Q, K, and V tensors into the format required for multi-head attention.
+ `def multi_head_attention`: It implements the core computation of multi-head attention. Performs parallel attention computation across multiple heads, capturing global dependencies for Transformer encoding.
+ `def positional_encoding`: It is used to add position information to input sequences or graph nodes in Transformers or Diffusion-based models. Notably, `positional_encoding_init` generates a reusable positional encoding table, while `positional_encoding_DIFUSCO` and `positional_encoding_ELG` generate encodings dynamically based on input.


## GNN

This module `class GNNEncoder` implements a configurable Graph Neural Network (GNN) encoder. It constructs a GNN backbone composed of multiple GNN layers, each performing node and edge feature updates with gated mechanisms, aggregation, residual connections, and normalization. The network supports both dense and sparse graph inputs and can incorporate positional and temporal embeddings to enhance relational reasoning over graph-structured data.

### `GNNEncoder`
```python
class GNNEncoder(
    n_layers: int = 6,
    hidden_dim: int = 128,
    out_channels: int = 1,
    aggregation: str = "sum",
    norm: str = "layer",
    learn_norm: bool = True,
    track_norm: bool = False,
    gated: bool = True,
    sparse: bool = False,
    use_activation_checkpoint: bool = False,
    node_feature_only: bool = False,
)
```

**Parameters**
+ `n_layers` (`int`): Number of stacked GNN layers.
+ `hidden_dim` (`int`): Dimension of node and edge hidden embeddings.
+ `out_channels` (`int`): Dimension of the output embeddings.
+ `aggregation` (`str`): Neighborhood aggregation scheme: *"sum"*, *"mean"*, or *"max"*.
+ `norm` (`str`): Feature normalization method: *"layer"*, *"batch"*, or *None*.
+ `learn_norm` (`bool`): Whether normalization has learnable parameters.
+ `track_norm` (`bool`): Whether to track running statistics in batch normalization.
+ `gated` (`bool`): Whether to use edge gating mechanism.
+ `sparse` (`bool`): Whether the input graph is represented in sparse format.
+ `use_activation_checkpoint` (`bool`): Whether to use activation checkpointing to save memory.
+ `node_feature_only` (`bool`): Whether to update only node features, ignoring edges.

**Attributes**
+ `node_embed` (`nn.Linear`): Linear embedding layer for node features.
+ `edge_embed` (`nn.Linear`): Linear embedding layer for edge features.
+ `pos_embed` (`nn.Module`): Positional embedding for nodes or edges (Sine-based).
+ `edge_pos_embed` (`nn.Module`): Positional embedding for edges (Sine-based scalar).
+ `time_embed` (`nn.Sequential`): Temporal embedding network for time-step features.
+ `layers` (`nn.ModuleList`): List of GNNLayer modules for stacked message passing.
+ `time_embed_layers` (`nn.ModuleList`): Per-layer time embedding transformations.
+ `per_layer_out` (`nn.ModuleList`): Per-layer edge feature output transformations.

**Methods**
+ `forward`: Performs multi-layer GNN encoding, updating node and edge features. Supports dense or sparse graph inputs. Returns edge embeddings or node embeddings depending on configuration.
+ `dense_forward`: Forward pass for dense graph representation.
+ `sparse_forward`: Forward pass for sparse graph representation.
+ `sparse_forward_node_feature_only`: Forward pass when only node features are used.
+ `sparse_encoding`: Internal function implementing sparse GNN message passing across all layers.

### Components

It consists of multiple layers, each composed of `GNNLayer + Residual + Normalization + Edge Gating`.

```python
for i in range(self.n_layers):
    h, e = self.layers[i](h, e, graph, mode="residual", edge_index=edge_index, sparse=sparse)
    if not self.node_feature_only:
        e = e + time_layer(time_emb)
    else:
        h = h + time_layer(time_emb)
    h = h_in + h
    e = e_in + out_layer(e)
```

+ **GNNLayer**: Performs message passing and gated edge updates.
+ **Node update**: Aggregates neighbor messages and adds residual connection.
+ **Edge update**: Combines source node, target node, and edge features with sigmoid gating.
+ **Supports aggregation**: sum / mean / max.
+ **Supports normalization**: batch / layer / None.
+ **Residual Connection**: Adds previous node/edge features to updated features.
+ **Normalization**: Stabilizes training by normalizing feature distributions.
+ **Edge Gating**: Controls the contribution of each edge in neighbor aggregation.


### `GNNLayer`
```python
class GNNLayer(
    hidden_dim: int,
    aggregation: str = "sum",
    norm: str = "batch",
    learn_norm: bool = True,
    track_norm: bool = False,
    gated: bool = True,
)
```

**Parameters**
+ `hidden_dim` (`int`): Dimension of hidden node and edge embeddings.
+ `aggregation` (`str`): Aggregation method: sum, mean, or max.
+ `norm` (`str`): Normalization type: batch, layer, or None.
+ `learn_norm` (`bool`): Whether normalization layers are learnable.
+ `track_norm` (`bool`): Whether batch statistics are used for batch normalization.
+ `gated` (`bool`): Whether to use gating mechanism for edges.

**Attributes**
+ `U`, `V`, `A`, `B`, `C` (`nn.Linear`): Linear layers for node and edge feature transformations.
+ `norm_h` (`nn.Module` or `None`): Node feature normalization layer.
+ `norm_e` (`nn.Module` or `None`): Edge feature normalization layer.

**Methods**
+ `forward`: Updates node and edge features using gated aggregation and residual connections.
+ `aggregate`: Aggregates neighbor messages according to the selected aggregation scheme.

### Other Utilities
+ `PositionEmbeddingSine`: Generates sine-based positional embeddings for node coordinates.
+ `ScalarEmbeddingSine`: Generates sine-based positional embeddings for scalar edge features.
+ `ScalarEmbeddingSine1D`: Generates sine-based positional embeddings for 1D scalar inputs.
+ `run_sparse_layer`: Wraps sparse GNN layer with residual and temporal embeddings.
+ `normalize`: Applies GroupNorm normalization.
+ `zero_module`: Zero-initializes module weights for residual branches.


## Diffusion

This module implements `Gaussian Diffusion` and `Categorical Diffusion` processes for generative modeling. It provides a structured way to add noise to data over a sequence of steps and defines methods for both forward sampling and posterior prediction, which are fundamental in denoising diffusion probabilistic models (DDPM) and their discrete variants.

### `GaussianDiffusion`
```python
class GaussianDiffusion(
    T: int,
    schedule: str
)
```

**Parameters**
+ `T` (`int`): Number of diffusion steps.
+ `schedule` (`str`): Type of noise schedule, supports *'linear'* or *'cosine'*.

**Attributes**
+ `beta` (`np.ndarray`): Noise level at each timestep.
+ `alpha` (`np.ndarray`): Alpha values for each step, alpha = 1 - beta.
+ `alphabar` (`np.ndarray`): Cumulative product of alpha over time.
+ `betabar` (`np.ndarray`): Cumulative product of beta over time (for convenience).

**Methods**
+ `sample` (`x0`, `t`) -> (`xt`, `epsilon`): Given input x0, returns a noisy version at timestep t along with the sampled Gaussian noise epsilon.
+ `posterior` (`target_t`, `t`, `pred`, `xt`, `inference_trick='ddim'`) -> `xt_target`: Computes the posterior distribution of x_{t-1} given x_t and a predicted noise pred. Supports DDPM and DDIM-style inference.

**Notes**
+ Supports linear and cosine noise schedules.
+ Designed for continuous data (Gaussian noise assumption).
+ inference_trick='ddim' allows deterministic transitions for faster sampling.

### `CategoricalDiffusion`
```python
class CategoricalDiffusion(
    T: int,
    schedule: str,
    sparse: bool = False
)
```

**Parameters**
+ `T` (`int`): Number of diffusion steps.
+ `schedule` (`str`): Noise schedule type, 'linear' or 'cosine'.
+ `sparse` (`bool`): Whether the data is treated as sparse for reshaping.

**Attributes**
+ `Qs` (`np.ndarray`): Transition matrices for each diffusion step.
+ `Q_bar` (`np.ndarray`): Cumulative product of transition matrices, representing the multi-step transition probabilities.

**Methods**
+ `sample` (`x0_onehot`, `t`) -> `xt`: Generates a noisy version of discrete input x0_onehot at step t using the cumulative transition matrix.
+ `posterior` (`target_t`, `t`, `x0_pred_prob`, `xt`, `guided=False`, `grad=None`) -> (`xt_target`, `probability`): Computes posterior probabilities for discrete diffusion. Supports optional guided sampling using gradients for conditional generation.

**Notes**
+ Works on discrete data (e.g., binary/categorical).
+ Supports guided diffusion via gradient signals.
+ Handles sparse reshaping to match probabilistic transitions for large discrete spaces.

**Core Concepts**

1. **Diffusion Steps (T)**
+ Defines how many times noise is added sequentially.
+ Both Gaussian and Categorical diffusion use T to construct cumulative transitions.

2. **Noise Schedule**
+ linear: Increases noise linearly across steps.
+ cosine: Uses a cosine-based cumulative alpha schedule for smoother transitions.

3. **Forward Sampling**
+ Continuous: xt = sqrt(alphabar_t) * x0 + sqrt(1 - alphabar_t) * epsilon.
+ Discrete: xt = x0_onehot @ Q_bar_t.

4. **Posterior Sampling**
+ Continuous: Computes x_{t-1} conditioned on current x_t and predicted noise.
+ Discrete: Uses matrix inversion on cumulative transition matrices to compute probabilities, optionally incorporating gradients for guidance.

5. **Sparsity Handling**
+ Categorical diffusion supports sparse reshaping for memory efficiency in large discrete domains.

### Usage Example
```python
# Gaussian Diffusion
gd = GaussianDiffusion(T=1000, schedule='linear')
xt, epsilon = gd.sample(x0, t=50)
x_prev = gd.posterior(target_t=None, t=50, pred=epsilon, xt=xt)

# Categorical Diffusion
cd = CategoricalDiffusion(T=1000, schedule='linear', sparse=True)
xt = cd.sample(x0_onehot, t=50)
xt_target, prob = cd.posterior(target_t=None, t=50, x0_pred_prob=x0_pred_prob, xt=xt, guided=True, grad=grad)
```