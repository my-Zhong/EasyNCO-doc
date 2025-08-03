# Backbones

## Navigation
- [Transformer](#transformer)
- [GNN](#gnn)
- [Diffusion](#diffusion)


## Transformer

该模块实现了一种基于多头注意力（Multi-Head Attention, MHA）的 Transformer 网络架构。构建由多层注意力机制与前馈网络组成的 Transformer 编码器结构，通过残差连接和归一化稳定训练过程，提升对图结构或序列数据的全局建模能力。

**Classes**
+ `MultiHeadAttentionLayer`: 实现标准的多头注意力机制，支持权重注入（用于元学习或模型外推）以及可选的 bias 设置。是 Transformer 类模型的核心组件之一。
+ `TransformerNet`
+ `Compatibility`
+ `SkipConnection`
+ `Normalization`
+ `FeedForward`


**Functions**
+ `reshape_by_heads`
+ `multi_head_attention`
+ `positional_encoding_DIFUSCO`
+ `positional_encoding_ELG`
+ `positional_encoding_init`

### `class` `MultiHeadAttentionLayer`

```python
class MultiHeadAttentionLayer(
    embed_dim: int,
    num_heads: int = 8,
    qkv_dim: int = 16,
    bias=False,
    bias3=False,
)
```

**Parameters**

+ embed_dim (int): 输入嵌入的维度。
+ num_heads (int): 注意力头的数量。
+ qkv_dim (int): 每个头的查询、键、值的维度。
+ bias (bool | list): 是否为 Q, K, V, output 添加偏置。可为布尔值或布尔列表。
+ bias3 (bool): 是否为输出层 multi_head_combine 添加偏置。

**Attributes**

+ Wq: 将输入嵌入（q_input）映射成多个注意力头的 Query 向量（查询向量）。
+ Wk: 将输入嵌入（kv_input）映射成多个注意力头的 Key 向量（键向量）。
+ Wv: 将输入嵌入（kv_input）映射成多个注意力头的 Value 向量（值向量）。
+ multi_head_combine: 将所有头的输出拼接后的结果合并回原始的嵌入维度。

**Methods**

+ `forward`: 执行完整的多头注意力计算，包括 query、key、value 的线性变换、head 的拆分、注意力打分和加权、以及最终的多头输出合并。




### `class` `TransformerNet`

```python
class TransformerNet(
    num_layers: int = 6,
    num_heads: int = 8,
    qkv_dim: int = 16,
    embed_dim: int = 128,
    normalization: str = "batch",
    feedforward_hidden: int = 512,
    bias=False,
    bias3=False,
)
```

**Parameters**

+ num_layers (int): Transformer 层的堆叠层数。
+ num_heads (int): 每层中多头注意力的头数。
+ qkv_dim (int): 每个注意力头的维度。
+ embed_dim (int): 输入嵌入和输出的维度。
+ normalization (str): 使用的归一化方式，支持 "batch"、"layer" 或 None。
+ feedforward_hidden (int): 前馈网络中间层的维度。
+ bias (bool): 是否在线性层中使用 bias。
+ bias3 (bool): 是否在 multi-head combine 层中使用 bias

**Attributes**

+ MHA_layers (nn.ModuleList): 多层的 MultiHeadAttentionLayer 模块，每层使用 SkipConnection 封装。
+ norm1 (nn.ModuleList): 每一层 attention 后的归一化模块。
+ FF_layers (nn.ModuleList): 前馈网络模块列表，每层封装在 SkipConnection 中。
+ norm2 (nn.ModuleList): 每一层前馈后归一化模块列表。

**Methods**

+ forward(input: torch.Tensor, weights=None): 对输入张量进行多层 Transformer 编码。若提供外部 weights 字典，则使用外部权重计算；否则使用模型自身权重。返回形状为 (batch, graph_size, embed_dim) 的张量。



### `class` `Compatibility`

```python
class Compatibility(
    embed_dim, 
    n_heads, 
    qkv_dim, 
    key_dim, 
    am_mode=True, 
    **kwargs
)
```

**Parameters**

+ embed_dim: 输入嵌入的维度。
+ n_heads: 注意力头的数量，仅在 am_mode=True 时使用。
+ qkv_dim: 每个注意力头中的 Q/K/V 的维度。
+ key_dim: 用于缩放打分的维度（一般为 qkv_dim）。
+ am_mode: 是否为切换为 Attention Model 模式。若为 True，则对输入进行线性投影生成 query 和 key。若为 False（POMO 模式），则直接使用原始嵌入。

**Attributes**

+ W_query: 若为 Attention Model 模式，对 query 向量进行线性变换。
+ W_key: 若为 Attention Model 模式，对 encoded_nodes 进行线性变换。

**Methods**

+ forward(self, q, encoded_nodes, mask=None, logit_clipping=10, penalty=None, **kwargs): 实现单头注意力机制，用于计算当前状态与所有候选节点之间的相似度得分。

### `class` `SkipConnection`





### `class` `Normalization`






### `class` `FeedForward`




### `def` `reshape_by_heads`



### `def` `multi_head_attention`


### `positional_encoding`

+ `positional_encoding_DIFUSCO`
+ `positional_encoding_ELG`
+ `positional_encoding_init`














































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











