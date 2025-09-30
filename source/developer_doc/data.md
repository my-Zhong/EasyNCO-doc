# Data

Our platform provides corresponding data for different problems. There are two methods of data generation:
+ **Random data generator**: Automatically creates randomized datasets based on specified size and parameters for quick experiments or benchmarking.
+ **Customized data loader**: Loads user-provided datasets from a specified path and file type for custom or real-world data usage.

The `class random_{problem}_generator` and `class customized_{problem}_loader` are integrated into the function `def {problem}Generator`, allowing users to select their preferred data generation method.

Please see individual problem descriptions for details.
+ Routing Problems: TSP, ATSP, PCTSP, CVRP, OP, SOP
+ Scheduling Problems: FFSP, RCPSP, SMTWTP
+ Packing Problems: KP, MKP, BPP
+ Other Graph Problems: MIS

## Random data generator

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`: Number of samples in the dataset.
+ `num_nodes`: Scale of the problem instance.
+ `device`: Device to store the data (CPU/GPU).

**Methods:**
+ `__getitem__`: Returns the samples.
+ `__len__`: Returns the total number of samples.

## Customized data loader

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`: Number of samples in the dataset.
+ `num_nodes`: Scale of the problem instance.
+ `device`: Device to store the data (CPU/GPU).
+ `path`: Path to the custom data file.
+ `file_type`: File type of the custom data file.

**Methods:**
+ `__getitem__`: Returns the samples.
+ `__len__`: Returns the total number of samples.
