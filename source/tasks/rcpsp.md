# Resource-Constrained Project Scheduling Problem

## Introduction

**Resource-Constrained Project Scheduling Problem (RCPSP)** is a classical model in project scheduling, where tasks are subject to precedence constraints and limited resources. The goal is to schedule the project while satisfying both resource limitations and task dependencies, typically to optimize the project duration.

## Problem Description
- **Entities**
  - **Task**: A project should consist of multiple tasks, each with a known duration.
  - **Resources**: There are various types of resources, each with a fixed total availability. 
- **Constraints**
  - Tasks are subject to precedence constraints (e.g., task B cannot start before task A finishes). Resources must be used within their fixed limits.
- **Objective**
  - Minimize the project makespan (total project duration).


## Data Generation

This module mainly provides a function `def RCPSPGenerator()`, which is used to generate a `DataLoader` object (see `data/RCPSPGenerator.py`).

+ Generate problem instances randomly (`class random_rcpsp_generator(Dataset)`).
+ Load custom data from a given file (`class customized_rcpsp_loader(Dataset)`).


### Random Data Generation
> This part is marked as *TODO* and will be implemented in a future update.

### Custom Data Loading

```python
class customized_rcpsp_loader(
  num_sample, 
  num_nodes, 
  device, 
  path
  )
```

**Bases:** `Dataset`

**Attributes:**
+ `num_sample`(`int`): Number of samples to load.
+ `num_nodes`(`int`): Number of nodes (jobs) in each project instance.
+ `device`(`str`, optional): Device to load tensors on ('cpu' or 'cuda'). Default is 'cpu'.
+ `path`(`str`, optional): Path to the directory containing `.RCP` files.
> 💡**Tips**: It currently supports files in `.RCP` format, which follow a standard format as used in PSPLIB instances.

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`.
+ `def readints(f) -> List[int]`: Reads a line from a file `f` and parses it into a list of integers.
+ `def read_RCPfile(filepath)`: Parses a single .RCP file and extracts RCPSP problem data. 
  + **Parameters:**
    + `filepath` (`str`): Path to the `.RCP` file.
  + **Returns:**
    + `resource_capacity` (`tensor`): 1D tensor of resource capacities, shape `(n_resources,)`.
    + `duration` (`tensor`): 1D tensor of job durations, shape `(n_jobs,)`.
    + `resources` (`tensor`): 2D tensor of resource demands per job, shape `(n_jobs, n_resources)`.
    + `succ_mat` (`tensor`): Successor matrix, 2D tensor of shape `(n_jobs + 1, n_jobs + 1)` where succ_mat[i, j] = 1 indicates job j is a successor of job i.
+ `def successor_mat_gen(n, r)`: Generates a successor matrix given the number of jobs and a list of successor relationships.
  + **Parameters:**
    + `n` (`int`): Number of jobs.
    + `r` (`list`): List of (predecessor, successor) pairs.
  + **Returns:**
    + `succ_mat` (`tensor`): Binary matrix of shape `(n+1, n+1)` marking successors.
