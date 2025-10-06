# Flexible Flow Shop Problem

## Introduction

The **Flexible Flow Shop Problem (FFSP)** is an extension of the traditional Flow Shop Scheduling Problem (FSP) by allowing multiple parallel machines at each stage, with each operation assigned to one of them.

### Problem Description

- **Entities**
  - **Job**: A job is a task that must be processed through all stages in a fixed order.
  - **Stage/Operation**: A stage represents one step in the processing sequence, each with a set of parallel machines.
  - **Machine**: A machine is a resource at a stage that can process one job at a time.
- **Constraints** 
  - Each job must go through all stages in order, and each stage can be processed on any available machine in that stage.
- **Objective**
  -  Minimize the makespan (maximum completion time).

## Data Generator

This module mainly provides a function `def FFSPGenerator()`, which is used to generate a `DataLoader` object (see `data/FFSPGenerator.py`).

+ Generate problem instances randomly (`class random_ffsp_generator()`).
+ Load custom data from a given file (`class customized_ffsp_loader(Dataset)`).

### Random Data Generator

```python
class random_ffsp_generator(
        num_sample,
        machine_cnt_list,
        job_cnt,
        time_low,
        time_high,
        device,
    )
```

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`(`int`): number of samples in the dataset
+ `machine_cnt_list`(`list`): list of machine counts per stage, e.g., `[4,4,4]` means 3 stages each with 4 machines
+ `job_cnt`(`int`): number of jobs
+ `time_low`(`int`): lower bound of processing time
+ `time_high`(`int`): upper bound of processing time
+ `device`(`str`): device to store the data (CPU/GPU)

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, idx) -> tensor`: randomly generates a tensor of shape `(job_cnt, total_machines)`, where each element is a processing time sampled uniformly from `[time_low, time_high]`


### Custom Data Loading

```python
class customized_ffsp_loader(
  num_sample, 
  device, 
  path)
```

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`(`int`): number of samples in the dataset
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor) format.

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`.

