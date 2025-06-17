# SMTWTP Problem and Data Generation

## Introduction to the SMTWTP Problem
The Single Machine Total Weighted Tardiness Problem(SMTWTP) is a scheduling problem that involves sequencing a set of jobs on a single machine to minimize the total weighted tardiness.

### Problem Description


- **Jobs with Attributes** : Each job  $J_i$ has a processing time $ p_i $, a due date $ d_i $, and a weight $ w_i $ representing its importance.

- **Single Machine**: All jobs must be processed non-preemptively on a single machine that can handle only one job at a time.

- **Tardiness Definition** : The tardiness of a job is defined as:

$$
T_i = \max(0, C_i - d_i)
$$
where $ C_i $ is the completion time of job $ J_i $.

- **Total Weighted Tardiness** : The total cost is the sum of weighted tardiness across all jobs:

$$
\sum_{i=1}^{n} w_i \cdot T_i
$$


- **Objective**:  Minimize the sum of the weighted tardiness of all jobs.


### Constraints
- **Single-Machine Constraint**: Only one job can be processed at a time.
- **Non-Preemptive Execution**: Once a job starts, it runs to completion without interruption.


## Data Generation with SMTWTPGenerator.py
The `SMTWTPGenerator.py` file provides a flexible way to generate data for the SMTWTP problem, supporting both random data generation and loading of customized data.

### Random Data Generation
The `random_smtwtp_generator` class within `SMTWTPGenerator.py` is designed to create random SMTWTP data.

- **`__init__`**: The class is initialized with parameters like the number of samples (`num_sample`), number of nodes (`num_nodes`), and the device (e.g., CPU or GPU).

- **`__getitem__`**: **`[due_time, weights, processing_time]`** are sampled as $t_i \sim n \times \mathrm{Uniform}(0, 1), \quad w_i \sim \mathrm{Uniform}(0, 1), \quad p_i \sim \mathrm{Uniform}(0, 1)$


### Custom Data Loading
For users with specific data requirements, the `customized_smtwtp_loader` class allows loading customized SMTWTP data from files.

- **File Support**: It supports files in `.pkl` (Python pickle) or `.pt` (PyTorch tensor) formats.
- **Data Loading**: The data is loaded and converted into a tensor format suitable for use with PyTorch.


### Data Feature
The output data has three dimensions:**`[due_time, weights, processing_time]`**
  - **due_time** : The target time by which a job should ideally be completed
  - **weights** :  A priority value representing the cost per unit time of tardiness for the job
  - **processing_time** : The amount of time required to complete a job 

### Using SMTWTPGenerator
The `SMTWTPGenerator` function serves as a convenient interface to either generate random data or load customized data based on the provided parameters. It returns a DataLoader object that can be used to efficiently load data in batches during model training or algorithm testing.

By utilizing `SMTWTPGenerator.py`, researchers and practitioners can easily prepare and access data for experimenting with and developing solutions for the SMTWTP problem.