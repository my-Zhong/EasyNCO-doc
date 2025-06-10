# SOP Problem and Data Generation

## Introduction to the SOP Problem
The SOP (Sequential Ordering Problem) is a combinatorial optimization problem that involves finding an optimal sequence of nodes to visit, considering specific order constraints.

### Problem Description
- **Nodes with Coordinates**: Each node has specific coordinates (e.g., (x, y) in a 2D plane).
- **Ordering Constraints**: Certain nodes must be visited before others.
- **Travel Cost**: The cost of traveling between nodes.
- **Objective**: Find the optimal path that minimizes the total travel cost while respecting the ordering constraints.

### Constraints
- **Ordering Constraints**: Specific nodes must be visited before others.
- **Path Continuity**: The path must be continuous, moving directly from one node to another.

## Data Generation with SOPGenerator.py
The `SOPGenerator.py` file provides a flexible way to generate data for the SOP problem, supporting both random data generation and loading of custom data.

### Random Data Generation
The `random_sop_generator` class within `SOPGenerator.py` is designed to create random SOP data.

- **Initialization**: The class is initialized with parameters like the number of samples (`num_sample`), number of nodes (`num_nodes`), and the device (e.g., CPU or GPU).
- **Data Creation**:
  - A cost matrix is generated using the `cost_mat_gen` function.
  - Ordering constraints are generated using the `ordering_constraint_gen` function.
  - An adjacency matrix is generated based on the ordering constraints using the `adjacency_mat_gen` function.
  - The generated data includes both the cost matrix and the adjacency matrix.

### Custom Data Loading
For users with specific data requirements, the `customized_sop_loader` class allows loading custom SOP data from files.

- **File Support**: It supports files in `.pkl` (Python pickle) or `.pt` (PyTorch tensor) formats.
- **Data Loading**: The data is loaded and converted into a tensor format suitable for use with PyTorch.

### Data Feature
The output data has three dimensions:
  - **Two dimensions for the cost matrix**, representing the travel costs between nodes.
  - **One dimension for the adjacency matrix**, indicating the ordering constraints between nodes.

### Using SOPGenerator
The `SOPGenerator` function serves as a convenient interface to either generate random data or load custom data based on the provided parameters. It returns a DataLoader object that can be used to efficiently load data in batches during model training or algorithm testing.

By utilizing `SOPGenerator.py`, researchers and practitioners can easily prepare and access data for experimenting with and developing solutions for the SOP problem.
