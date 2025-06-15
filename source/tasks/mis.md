# Maximum Independent Set Problem

## Introduction

The Maximum Independent Set (MIS) problem seeks the largest set of vertices in an undirected graph such that no two vertices in the subset are connected by an edge.


### Problem Description

- **Entities**
  - **Undirected Graph**: An undirected graph consists of a set of vertices and a set of edges, where the edges have no directionality and simply represent connections between vertices.
  - **Vertex**: Represents an entity or element in the problem domain
  - **Edge**: Represents a relationship or conflict between two vertices
- **Constraints** 
  - No two vertices in the independent set can be connected by an edge (adjacent).
- **Objective**
  -  Find the independent set with the maximum number of vertices.


## Data Generator

This module mainly provides a function `def MISGenerator()`, which is used to generate a `DataLoader` object (see `data/MISGenerator.py`).

+ Generate problem instances randomly (`Random generation for MIS is not supported currently`).
+ Load custom data from a given file (`class cutomized_mis_loader(Dataset)`).

### Random Data Generator
**`Random generation for MIS is not supported currently`**


### Custom Data Loading
**`class customized_mis_loader()`**

**Attributes:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_node`(`int`): number of nodes in a instance
+ `device`(`str`): device to store the data (CPU/GPU)
+ `path`(`str`): the specified path of custom data
+ `file_type`(`str`): the file type
+ `graph`(`bool`): a flag to identify graph data files
> 💡**Tips**: It currently supports files in `.pkl` (Pickle) format.


**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> tensor`: loads data from the specified `index`.

### Data Feature
The output data is a tuple containing three elements
  - **Index** : the index of the current graph.
  - **Graph Data** : indicating node features and edge structure.
  - **Node indicator** : indicating the node count of the current graph.
