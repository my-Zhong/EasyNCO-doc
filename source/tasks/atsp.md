## Introduction
The Asymmetric Traveling Salesman Problem (ATSP) seeks to find the shortest possible route that visits a set of cities exactly once and returns to the origin, with the defining characteristic that the travel cost from city A to city B may be different from the cost of returning from B to A.

### Problem Description
- **Entities**
  - **Distance matrix**: A matrix that records the travel distance between different cities.
- **Constraints**
  - The tour starts and ends at the same cities.
  - Each cities must be visited exactly once.
- **Objective**
  - Minimizing the total travel distance.

## Data Generator
This module mainly provides a function `def ATSPGenerator()`, which is used to generate a `DataLoader` object (see `data/ATSPGenerator`).
+ Generate problem instances randomly (`class randon_atsp_generator()`).
+ Load custom data from a given file (`class customized_atsp_loader(Dataset)`).

### Random Data Generator
**`class random_atsp_generator()`**

**Attributes**
+ `num_sample`(`int`): number of samples in the dataset.
+ `num_nodes`(`int`): number of cities.
+ `int_min`: The lowest integer that can possibly be generated. (default = 0)
+ `int_max`: The upper bound for the generation. (default = 1000000)
+ `scaler`: This scaler normalizes the distance matrix by mapping its values to the range [0, 1].
+ `device`:device to store the data (CPU/GPU).



**<font color='red'>Functions </font>:**
+ `__len__(self) -> int`: returns the total number of samples.
+ `__getitem__(self) -> tensor`:randomly generate a tensor of shape `(num_nodes, num_nodes)` representing the distance matrix of the cities.


### <font color='red'>Custom DataLoader</font>
+ `num_sample(`int`)`: number of instances in the dataset.
+ `num_nodes(`int`): number of cities.
+ `device`(`str`): device to sotre the data (CPU/GPU).
+ `path`(`str`): path the custom data.

> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor).

**Functions**:
+ `__len__(self) -> int`: returns the total number of samples.
+ `__getitem(self, index) -> tensor`: loads data from the specified `index`.