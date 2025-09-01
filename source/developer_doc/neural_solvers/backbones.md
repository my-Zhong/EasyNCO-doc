# Backbones
Our platform integrates three mainstream deep learning backbones to accommodate various NCO solvers.

- [Transformer](#transformer)
- [GNN](#gnn)
- [Diffusion](#diffusion)

## Transformer

This module `class TransformerNet` implements a Transformer architecture based on Multi-Head Attention (MHA). It constructs a Transformer backbone composed of multiple layers of attention mechanisms and feed-forward networks, and leverages residual connections and normalization to stabilize the training process, thereby enhancing the capability of global modeling over graph-structured or sequential data.

### `TransformerNet`
```python
class TransformerNet(
    num_layers: int = 6,
    num_heads: int = 8,
    qkv_dim: int = 16,
    embed_dim: int = 128,
    normalization: str = "batch",  # "batch", "instance", None
    feedforward_hidden: int = 512,
    bias: bool = False,  # bias for Wq
    bias_k: bool = None,  # bias for Wk, if None, use bias for Wk
    bias_v: bool = None,  # bias for Wv, if None, use bias for Wv
    bias_combine: bool = True,  # bias for multi_head_combine
    )
```

**Parameters**
+ num_layers (int): Number of stacked Transformer layers.
+ num_heads (int): Number of attention heads in each layer.
+ qkv_dim (int): Dimension of each attention head.
+ embed_dim (int): Dimension of the input embeddings and outputs.
+ normalization (str): Type of normalization to use, supports "batch", "layer", or None.
+ feedforward_hidden (int): Dimension of the hidden layer in the feed-forward network.
+ `bias_k`, `bias_v`, `bias_combine` (`bool`): Whether to add bias to K, V and output.
+ `multi_head_combine_used` (bool): Whether to use the output layer multi_head_combine.

**Attributes**
+ MHA_layers (nn.ModuleList): Multi-layer MultiHeadAttentionLayer modules, each wrapped with a SkipConnection.
+ norm1 (nn.ModuleList): Normalization modules applied after attention in each layer.
+ FF_layers (nn.ModuleList): Feed-forward network modules, each wrapped in a SkipConnection.
+ norm2 (nn.ModuleList): Normalization modules applied after the feed-forward network in each layer.

**Methods**
+ forward(input: torch.Tensor, weights=None): Performs multi-layer Transformer encoding on the input tensor. If an external weights dictionary is provided, it uses the external weights for computation; otherwise, it uses the model’s own parameters. Returns a tensor of shape (batch, graph_size, embed_dim).

### Components
It consists of multiple layers, each composed of `MHA layer + Norm + FFN + Norm`.
```python
for i in range(self.num_layers):
    out = self.MHA_layers[i](out)
    out = self.norm1[i](out)
    out = self.FF_layers[i](out)
    out = self.norm2[i](out)
```
+ `MultiHeadAttentionLayer`: It with `SkipConnection` constitutes the main part of the multi-head attention mechanism, supporting weight loading and optional bias settings.
+ `Normalization`: It supports three types of normalization: **batch**, **instance**, and **None**.
+ `FeedForward`: It consists of two fully connected layers with a non-linear activation function (ReLU or GELU).
+ Another `Normalization`


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
+ embed_dim (int): Dimension of the input embeddings.
+ num_heads (int): Number of attention heads.
+ qkv_dim (int): Dimension of the query, key, and value vectors for each head.
+ `bias_k`, `bias_v`, `bias_combine` (`bool`): Whether to add bias to K, V and output.
+ `multi_head_combine_used` (bool): Whether to use the output layer multi_head_combine.

**Attributes**
+ Wq: Maps input embeddings (q_input) to query vectors for multiple attention heads.
+ Wk: Maps input embeddings (kv_input) to key vectors for multiple attention heads.
+ Wv: Maps input embeddings (kv_input) to value vectors for multiple attention heads.
+ multi_head_combine: Merges the concatenated outputs of all heads back into the original embedding dimension.

**Methods**
+ forward: Performs the full multi-head attention computation, including linear projections for queries, keys, and values, head splitting, attention scoring and weighting, and the final multi-head output combination

### Other utils
+ `class Compatibility`: Computes the compatibility score between the current query state and the remaining candidate nodes, producing a probability distribution for action selection. It is used in the RL-based NCO models, such as Attention Model and POMO Model.
+ `def reshape_by_heads`: It is used to reshape the linearly projected Q, K, and V tensors into the format required for multi-head attention.
+ `def multi_head_attention`: It implements the core computation of multi-head attention. Performs parallel attention computation across multiple heads, capturing global dependencies for Transformer encoding.
+ `def positional_encoding`: It is used to add position information to input sequences or graph nodes in Transformers or Diffusion-based models. Notably, `positional_encoding_init` generates a reusable positional encoding table, while `positional_encoding_DIFUSCO` and `positional_encoding_ELG` generate encodings dynamically based on input.














































## GNN

### GNN for DIFUSCO


### GNN for GLOP/UDC/DeepACO

### `EmbNet`
从图的节点特征和边特征中提取深层次的边嵌入（edge embedding），为后续预测提供丰富的图结构信息。

#### Parameters
depth: 网络层数，控制消息传递迭代次数。

feats: 节点特征维度。

edge_feats: 边特征维度。

units: 每层隐藏单元数（隐藏维度）。

act_fn: 激活函数名称，默认用 silu。

agg_fn: 邻居特征聚合方式，默认用 mean 池化。

#### Attributes

节点和边线性变换层（v_lin0, e_lin0）和多层线性层（v_lins1~4, e_lins0）

批归一化层（BatchNorm）用于稳定训练

聚合函数使用了PyG的全局池化，比如 global_mean_pool，用于邻居节点信息聚合。


#### Methods

每一层都进行节点和边的更新。

节点更新中，邻居节点特征乘以边权重 w2（用Sigmoid限制在[0,1]），聚合后加入自身特征残差。

边更新中结合了两端节点信息。

通过多层迭代，实现复杂的节点和边表示学习。

<details> 
    <summary>Expand to view code in .py.</summary>
    <pre><code>
def forward(self, x, edge_index, edge_attr):
    x = x
    w = edge_attr
    x = self.v_lin0(x)
    x = self.act_fn(x)
    w = self.e_lin0(w)
    w = self.act_fn(w)
    for i in range(self.depth):
        x0 = x
        x1 = self.v_lins1[i](x0)
        x2 = self.v_lins2[i](x0)
        x3 = self.v_lins3[i](x0)
        x4 = self.v_lins4[i](x0)
        w0 = w
        w1 = self.e_lins0[i](w0)
        w2 = torch.sigmoid(w0)
        x = x0 + self.act_fn(self.v_bns[i](x1 + self.agg_fn(w2 * x2[edge_index[1]], edge_index[0])))
        w = w0 + self.act_fn(self.e_bns[i](w1 + x3[edge_index[0]] + x4[edge_index[1]]))
    return x, w
    </code></pre> 
</details>










## Diffusion











