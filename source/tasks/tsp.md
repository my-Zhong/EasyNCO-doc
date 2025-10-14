## Introduction
The Traveling Salesman Problem (TSP) seeks to find the shortest possible route that visits a set of cities and returns to the starting city, visiting each city exactly once.

### Problem Description
- **Entities**
  - **City**: A location that needs to be visited.
- **Constraints**
  - The tour starts and ends at the same cities.
  - Each cities must be visited exactly once.
- **Objective**
  - Minimizing the total travel distance.

## Data Generator
This module mainly provides a function `def TSPGenerator()`, which is used to generate a `DataLoader` object (see `data/TSPGenerator`).
+ Generate problem instances randomly (`class randon_tsp_generator()`).
+ Load custom data from a given file (`class customized_tsp_loader(Dataset)`).

### Random Data Generator
**`class random_tsp_generator()`**
```python
class random_tsp_generator(Dataset):
    '''
    The random_tsp_generator is used to generate the random TSP data.
    The data is generated randomly with the size of [num_sample, num_nodes, 2]
    By default, the data is generated with the uniform distribution.
    '''
    def __init__(self, 
                 num_sample, 
                 num_nodes, 
                 device='cpu',
                 distribution="uniform")
```
**Attributes**
+ `num_sample`(`int`): number of samples in the dataset.
+ `num_nodes`(`int`): number of cities.
+ `device`(`str`): device to store the data (CPU/GPU).
+ `distribution` (`str`): <font color='red'>distribution of cities.</font>


**Functions:**
+ `__len__(self) -> int`: returns the total number of samples.
+ `__getitem__(self) -> tensor`:randomly generate a tensor of shape `(num_nodes, 2)` representing the coordinates of the cities.


### Custom DataLoader
`class customized_tsp_loader`
```python
class customized_tsp_loader(Dataset):
    '''
    The customized_tsp_loader is used to load the customized TSP data from the file.
    The file should be saved in the format of .pkl or .pt.
    And the data should be saved in the format of {'node_xy': shape (sample_num, problem, 2),
                                                    (if learning paradigm is sl, 'label' will be contained)}
    In the future, we will support the more general type of data.
    '''
    def __init__(self, num_sample, 
                 num_nodes, 
                 device='cpu', 
                 path=None, 
                 graph = False, 
                 **kwargs)
```

+ `num_sample(`int`)`: number of instances in the dataset.
+ `num_nodes(`int`): number of cities.
+ `device`(`str`): device to sotre the data (CPU/GPU).
+ `path`(`str`): path the custom data.
+ `graph`(`bool`): whether transform the data into graph (only support `.txt` file).

> 💡**Tips**: It currently supports files in `.pt` (PyTorch tensor), `.pkl` (Python pickle), and '.txt' format.

**Functions**:
+ `__len__(self) -> int`: returns the total number of samples.
+ `__getitem(self, index) -> tensor`: loads data from the specified `index`.