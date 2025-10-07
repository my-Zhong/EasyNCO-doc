
# Multiple Knapsack Problem 

## Introduction
The **Multiple Knapsack Problem (MKP)** is a generalization of the classic Knapsack Problem, where the goal is to distribute a set of items into multiple knapsacks, each with its own capacity constraint, to maximize the total value of the items assigned.

### Problem Description
- **Entities**
  - **weight**: The weight of an item represents how much space or load it occupies in a knapsack. It is a non-negative integer.
  - **value**:  The value of an item represents its worth or utility. It is also a non-negative integer.
  - **capacity**: The maximum weight that a knapsack can carry. This is a non-negative integer that limits the total weight of the items assigned to that knapsack.
- **Constraints** 
  - The sum of the weights of the items assigned to each knapsack must not exceed its capacity.
  - Each item can be assigned to at most one knapsack. 

- **Objective**
  - Maximize the total value of the items assigned to all knapsacks.



## Data Generator

This module mainly provides a function `def MKPGenerator()`, which is used to generate a `DataLoader` object (see `data/MKPGenerator.py`).

+ Generate problem instances randomly (`class random_mkp_generator()`).
+ Load custom data from a given file (`class customized_mkp_loader(Dataset)`).

### Random Data Generator
```python
class random_mkp_generator(
  num_sample, 
  num_nodes, 
  device
  )
```

**Bases:** `Dataset`

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `knapsacks`(`int`):  number of knapsacks in a instance.
+ `device`(`str`): device to store the data (CPU/GPU)
+ `constraints`(`int`): number of constraints

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, idx) -> tensor`: randomly generates a tensor of shape `(num_items, 1+constraints)`,where the first column represents the prizes for each knapsack and the remaining columns represent the normalized weights for each constraint.

### Custom Data Loading

```python
class customized_mkp_loader(
  num_sample, 
  num_nodes, 
  device, 
  path
  )
```

**Bases:** `Dataset`

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of items in a instance
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
+ `data`(`tensor`): the data is saved as a tensor with shape `(num_sample, num_nodes, 1+constraints)`
> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor), `.pkl`(Python pickle) format.

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`
