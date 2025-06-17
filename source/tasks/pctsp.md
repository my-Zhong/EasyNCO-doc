## Introduction

The Prize Collecting Traveling Salesperson Problem (PCTSP) is a variant of the Traveling Salesperson Problem where the objective is to find a tour that visits a subset of nodes to collect prizes, minimizing the sum of travel costs and penalties for unvisited nodes, while potentially satisfying a minimum collected prize threshold.


### Problem Description

- **Entities**
  - **Node**: Each node has specific coordinates (e.g., (x, y) in a 2D plane) 
  - **prize**: prize associated with visiting a specific node, collected if the node is visited.
  - **penalty**: A cost incurred for not visiting a specific node.
  - **traval cost**:  The cost of traveling between nodes.
- **Constraints** 
  - The tour must end at the start Node.
  - The total prize collected from visited nodes must meet a predefined minimum quota.
  - The nodes could be only visited once.
- **Objective**
  -  Minimize the sum of the total travel cost and the total penalty for unvisited nodes.   

## Data Generator

This module mainly provides a function `def PCTSPGenerator()`, which is used to generate a `DataLoader` object (see `data/PCTSPGenerator.py`).

+ Generate problem instances randomly (`class random_pctsp_generator()`).
+ Load custom data from a given file (`class customized_pctsp_loader(Dataset)`).

### Random Data Generator
**`class random_pctsp_generator()`**


**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of nodes
+ `device`(`str`): device to store the data (CPU/GPU)


**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, idx) -> tensor`: randomly generates a tensor of shape `(num_nodes, 4)`, `4` represents `2D_coordinates + prize + penalty`, where each prize and penalty in nodes is sampled uniformly.


### Custom Data Loading
**`class customized_pctsp_loader()`**

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_node`(`int`): number of nodes
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
> 💡**Tips**: It currently supports files in `.pkl`(Python pickle)  format.


**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`.
