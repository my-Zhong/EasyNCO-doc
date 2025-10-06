# Knapsack Problem

## Introduction
The **Knapsack Problem (KP)** is a classic combinatorial optimization problem where the goal is to select a subset of items with maximum total value, without exceeding a given weight capacity constraint.

### Problem Description
- **Entities**
  - **weight**: The weight of an item represents how much space or load it occupies in the knapsack. It is a non-negative integer.
  - **value**: The value of an item represents its worth or utility. It is also a non-negative integer.
  - **capacity**: he maximum weight that the knapsack can carry. This is a non-negative integer that limits the total weight of the selected items.

- **Constraints** 
  - Sum of selected items' weights ≤ capacity  
  - 0-1 selection policy (each item selectable at most once)  

- **Objective**
  - Maximize total value of selected items



## Data Generator

This module mainly provides a function `def KPGenerator()`, which is used to generate a `DataLoader` object (see `data/KPGenerator.py`).

+ Generate problem instances randomly (`class random_kp_generator()`).
+ Load custom data from a given file (`class customized_kp_loader(Dataset)`).

### Random Data Generator

```python
class random_kp_generator(
  num_sample, 
  num_items, 
  device
  )
```

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_items`(`int`):  number of items in a instance
+ `device`(`str`): device to store the data (CPU/GPU)

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, idx) -> tensor`: randomly generates a tensor of shape `(num_items, 2)`, where the first column represents the weights of the items, and the second column represents the values of the items. Both weights values and are uniformly sampled from the range [0, 1]

### Custom Data Loading

```python
class customized_kp_loader(
  num_sample, 
  num_items, 
  device, 
  path)
```

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`(`int`): number of instances in the dataset
+ `num_items`(`int`): number of items in a instance
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
+ `data`(`tensor`): the data is saved as a tensor with shape `(num_sample, num_items, 2)`
> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor), `.pkl`(Python pickle) format.

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`.
