# Bin Packing Problem

## Introduction

The **Bin Packing Problem (BPP)** is a classic combinatorial NP-hard optimization problem. It involves packing a set of items of different sizes into a finite number of bins of a fixed capacity in a way that minimizes the number of bins used.

### Problem Description

- **Entities**
  - **Item**: An item is an object with a specific size or weight that needs to be placed into a bin.
  - **Bin**: A bin is a container with a fixed capacity that can hold one or more items, as long as the sum of their sizes does not exceed the capacity.
- **Constraints**
  - Each item must be placed into exactly one bin.
  - The total size of the items in any given bin cannot exceed the bin's capacity.
- **Objective**
  - Minimize the total number of bins used to pack all items.

## Data Generator

This module mainly provides a function `def BPPGenerator()`, which is used to generate a `DataLoader` object (see `data/BPPGenerator.py`).

- Generate problem instances randomly (`class random_bpp_generator()`).
- Load custom data from a given file (`class customized_bpp_loader()`).

### Random Data Generator

A class to generate random Bin Packing Problem instances. Data is generated on-the-fly.

```python
class random_bpp_generator(
  num_sample,
  num_nodes,
  device
  )
```

**Bases:** `Dataset`

**Attributes:**
- `num_sample` (`int`): Number of samples in the dataset.
- `num_nodes` (`int`): Number of items to be packed in each sample.
- `device` (`str`): The device to store the data on (e.g., 'cpu'/'cuda').

**Methods:**
- `__len__(self) -> int`: Returns the total number of samples.
- `__getitem__(self, idx) -> torch.Tensor`: Randomly generates a single problem instance. It returns a `torch.Tensor` of shape `(num_nodes + 1,)`. The first element is always 0, and the subsequent `num_nodes` elements represent item sizes, which are integers sampled uniformly from the range [20, 100].

### Custom Data Loading

A class to load BPP problem instances from a specified data file.

```python
class customized_bpp_loader(
  num_sample, 
  num_nodes, 
  device, 
  path
  )
```

**Bases:** `Dataset`

**Attributes:**
- `num_sample` (`int`): The number of samples to use from the data file.
- `num_nodes` (`int`): The number of items in each sample.
- `device` (`str`): The device to store the data on.
- `path` (`str`): The path to the custom data file.
- `file_type` (`str`): The file extension of the data file (e.g., `.pkl`, `.pt`).
> 💡 **Tip**: It currently supports files in `.pt` (PyTorch tensor) and `.pkl` (Pickle) formats. The data within the file is expected to be a list or tensor of problem instances.

**Methods:**
- `__len__(self) -> int`: Returns the total number of samples.
- `__getitem__(self, index) -> torch.Tensor`: Loads and returns a single data instance as a tensor from the file at the specified `index`.