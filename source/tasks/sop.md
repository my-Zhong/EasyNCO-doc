
# Sequential Ordering Problem (SOP)

## Introduction

The Sequential Ordering Problem (SOP) involves finding a route that starts and ends at a depot, visiting a set of customers in a specific sequence to minimize the total travel cost while satisfying precedence constraints.

### Problem Description

- **Entities**
  - **Depot**: The starting and ending point for the route.
  - **Customer**: A location that must be visited.
  - **Precedence Constraints**: Define the order in which customers must be visited.
  - **Travel Cost**: The cost of traveling between locations.
- **Constraints** 
  - The route must start and end at the depot.
  - Precedence constraints must be satisfied.
- **Objective**
  - Minimize the total travel cost.

## Data Generator

This module mainly provides a function `def SOPGenerator()`, which is used to generate a `DataLoader` object (see `SOPGenerator.py`).

+ Generate problem instances randomly (`class random_sop_generator()`).
+ Load custom data from a given file (`class customized_sop_loader()`).

### Random Data Generator
**`class random_sop_generator()`**

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of customers
+ `device`(`str`): device to store the data (CPU/GPU)

**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: randomly generates a tensor that includes distance matrix and adjacency matrix. Distance matrix represents the travel cost between locations, and adjacency matrix represents the precedence constraints.

### Custom Data Loading
**`class customized_sop_loader()`**

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of customers
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor), `.pkl`(Python pickle) format.

**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`. The data includes distance matrix and adjacency matrix.
