# Capacitated Vehicle Routing Problem

## Introduction

The **Capacitated Vehicle Routing Problem (CVRP)** seeks to find the optimal set of routes for a fleet of vehicles to serve a set of customers with known demands, minimizing total travel cost while respecting vehicle capacity constraints.

### Problem Description

- **Entities**
  - **Depot**: The location where all vehicles start and end their routes. 
  - **Customer**: A location that needs to be visited.
  - **Vehicle**: A vehicle that visits customers, starting and ending at the depot.
  - **Demand**: The quantity of goods required by a customer.
  - **Capacity**: The maximum load a vehicle can carry.
- **Constraints** 
  - Each vehicle must start and end at the depot.
  - Each customer must be visited exactly once by exactly one vehicle.
  - The total demand of customers assigned to a vehicle cannot exceed its capacity.
- **Objective**
  -  Minimize the total travel distance for all vehicles.

## Data Generator

This module mainly provides a function `def CVRPGenerator()`, which is used to generate a `DataLoader` object (see `data/CVRPGenerator.py`).

+ Generate problem instances randomly (`class random_vrp_generator()`).
+ Load custom data from a given file (`class customized_vrp_loader(Dataset)`).

### Random Data Generator

```python
class random_vrp_generator(
  num_sample, 
  num_nodes, 
  demand_scaler, 
  device,
  **kwargs
  )
```

**Bases:** `Dataset`

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of customers
+ `demand_scaler`(`int`): decide the range of demand
+ `device`(`str`): device to store the data (CPU/GPU)

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, idx) -> tensor`: randomly generates a tensor of shape `(num_nodes + 1, 3)`, `1` represents the depot, where each demand in customers is sampled uniformly from `[1, 10]/demand_scaler` and the depot's demand is set to 0.


### Custom Data Loading

```python
class customized_vrp_loader(
  num_sample, 
  num_nodes, 
  device, 
  path
  )
```

**Bases:** `Dataset`

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `node`(`int`): number of customers
+ `data`(`dict`): the data should be saved in the format of {'depot_xy': shape `(num_sample, 1, 2)`,
                                                    'node_xy': shape `(num_sample, num_nodes, 2)`,
                                                    'node_demand': shape `(num_sample, num_nodes)`,
                                                    'label': (optional)}
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor), `.pkl`(Python pickle) format.

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`.
