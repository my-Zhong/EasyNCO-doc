# Orienteering Problem (OP)

## Introduction

The Prize-Collecting Orienteering Problem (OP) aims to identify the optimal route for a vehicle to maximize the total collected prize while not exceeding a given travel cost limit. The vehicle starts and ends at a depot, and each customer has a specific prize and a travel cost associated with visiting it.

### Problem Description

- **Entities**
  - **Depot**: The starting and ending point for the vehicle.
  - **Customer**: A location that can be visited to collect a prize.
  - **Prize**: The reward associated with visiting a customer.
  - **Travel Cost**: The cost of traveling between locations.
- **Constraints** 
  - The vehicle must start and end at the depot.
  - The total travel cost must not exceed the given limit.
- **Objective**
  - Maximize the total collected prize.

## Data Generator

This module mainly provides a function `def OPGenerator()`, which is used to generate a `DataLoader` object (see `OPGenerator.py`).

+ Generate problem instances randomly (`class random_op_generator()`).
+ Load custom data from a given file (`class customized_op_loader()`).

### Random Data Generator
**`class random_op_generator()`**

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of customers
+ `device`(`str`): device to store the data (CPU/GPU)

**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: randomly generates a tensor of shape `(num_nodes, 3)`. Each customer's coordinates are sampled uniformly from `[0, 1]`, and the prize is calculated based on the distance from the depot. The depot is included as the first node.

### Custom Data Loading
**`class customized_op_loader()`**

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of customers
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor), `.pkl`(Python pickle) format.

**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`. The prize is calculated based on the distance from the depot for each customer.
