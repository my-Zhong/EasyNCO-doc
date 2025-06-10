# OP Problem and Data Generation

## Introduction to the OP Problem
The OP (Orienteering Problem) is a combinatorial optimization problem that focuses on finding a path that visits a subset of nodes to maximize the total collected prize while not exceeding a given travel cost limit.

### Problem Description
- **Nodes with Coordinates and Prizes**: Each node has specific coordinates (e.g., (x, y) in a 2D plane) and an associated prize value.
- **Travel Cost**: The cost of traveling between nodes, often calculated using methods like Euclidean distance.
- **Objective**: Maximize the total prize collected by visiting nodes within the travel cost limit.

### Constraints
- **Travel Cost Limit**: The total travel cost of the path must not exceed a predefined threshold.
- **Path Continuity**: The path must be continuous, moving directly from one node to another.

## Data Generation with OPGenerator.py
The `OPGenerator.py` file provides a flexible way to generate data for the OP problem, supporting both random data generation and loading of custom data.

### Random Data Generation
The `random_op_generator` class within `OPGenerator.py` is designed to create random OP data.

- **Initialization**: The class is initialized with parameters like the number of samples (`num_sample`), number of nodes (`num_nodes`), and the device (e.g., CPU or GPU).
- **Data Creation**:
  - For each sample, random coordinates for nodes are generated using a uniform distribution.
  - Distances from each node to the starting node (depot) are calculated.
  - Prizes for the nodes are determined based on these distances, normalized to ensure the start node's prize is zero.
  - The generated data for each node includes its coordinates and prize value.

### Custom Data Loading
For users with specific data requirements, the `customized_op_loader` class allows loading custom OP data from files.

- **File Support**: It supports files in `.pkl` (Python pickle) or `.pt` (PyTorch tensor) formats.
- **Data Loading**: The data is loaded and converted into a tensor format suitable for use with PyTorch.

### Data Feature
The output data has three dimensions:
  - **Two dimensions for the Coordinates**, : representing the x and y positions of the node.
  - **One dimension for the Prize**, indicating the prize associated with the node.


### Using OPGenerator
The `OPGenerator` function serves as a convenient interface to either generate random data or load custom data based on the provided parameters. It returns a DataLoader object that can be used to efficiently load data in batches during model training or algorithm testing.

By utilizing `OPGenerator.py`, researchers and practitioners can easily prepare and access data for experimenting with and developing solutions for the OP problem.
