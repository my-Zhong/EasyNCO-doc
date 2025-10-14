# Multi_task Vehicle Routing Problem

## Introduction

The **Multi_task Vehicle Routing Problem (MVRP)** seeks to find the optimal set of routes for a fleet of vehicles to 
serve a set of customers with known demands, minimizing total travel cost under the premise of respecting various constraints
(e.g., time, distance).

### Problem Description

- **Entities**
  - **Depot**: The location where all vehicles start and end their routes. 
  - **Customer**: A location that needs to be visited.
  - **Vehicle**: A vehicle that visits customers, starting and ending at the depot.
  - **Demand**: The quantity of goods required by a customer.
  - **Capacity**: The maximum load a vehicle can carry.
  - **Time**:The time used by the current sub-route
  - **Length**:The distance traveled by the current sub-route
- **Constraints** 
  - Each vehicle must start and end at the depot.
  - Each customer must be visited exactly once by exactly one vehicle.
  - The total demand of customers assigned to a vehicle cannot exceed its capacity.
  - **Backhaul constraints**:Customers have Linehaul (deliver) and Backhaul (collect) tasks. Vehicles need to consider not only delivering 
  goods to customers but also transporting customers’ goods back to the depot.
  - **Open route constraints**: Every sub-route does not need to return to the depot.
  - **Time constraints**:Each customer has its own time window and required service duration, and vehicles must arrive 
  at the customer within the corresponding time window to provide service.
  - **Route limit constraints**:The length of each sub-route must not exceed the route limit.
- **Objective**
  -  Minimize the total travel distance for all vehicles.


## Data Generator

This module mainly provides a function `def MVRPGenerator()`, which is used to generate a `DataLoader` object (see `data/MVRPGenerator.py`).

+ Generate problem instances randomly (`class random_vrp_generator()`).
+ Load custom data from a given file (`class customized_vrp_loader(Dataset)`).

### Random Data Generator

```python
class random_vrp_generator(
    num_sample, 
    num_nodes, 
    demand_scaler,
    batch_size,
    mode,
    train_problems,
    device,
    **kwargs
  )
```

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of customers
+ `demand_scaler`(`int`): decide the range of demand
+ `batch_size`(`int`): batch_size of the data
+ `mode`(`int`): different time window generation methods: when mode = 0, it is the generation method of MTPOMO; 
when mode = 1, it is the generation method of MVMoE.
+ `train_problems`(`list`): a list of different problems, such as ["CVRP", "OVRP", "VRPB", "VRPL", "VRPTW", "OVRPTW"]
+ `device`(`str`): device to store the data (CPU/GPU)

**Methods:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> dict`:
randomly select a problem from train_problems, 
if the problem includes **Backhaul constraints**, 20% of the customers's demand 
will be set to their corresponding opposites.
If the problem includes the **Route_limit** constraint, set route_limit = 3.0.
If the problem includes **Time constraints**, the corresponding generation method will be selected based on the value of mode.
When mode = 0 (MTPOMO):
    * The service time $s_i$ and time window $Δ_i$ are randomly generated within the range $[0.15,0.2]$
    * The early_time is calculated by the formula: $e_i=\frac{h_i×d_{0i}}{v}$, 
where $d_{0i}$ represents the distance from node $i$ to the depot, $v$ denotes the speed, $T=4.6$ is maximum time interval of the depot,
$h_i$ is within the range $h_i∈[1,\frac{T-s_i-Δ_i}{d_{0i}}×v-1]$,
    * The late_time $l_i$ is calculated by the formula: $l_i=e_i+Δ_i$
  
  when mode = 1 (MVMoE):
  * The service time $s_i=0.2$, depot start_time $e_0=0$,depot end_time $l_0=3.0$. 
  * Sample time window center $γ_i$ \~ $U(e_0+d_{0i},l_0-d_{i0}-s_i)$
  * Sample time window hald-width $h_i$ \~ $U[s_i/2,l_0/3]$
  * Time window $[e_i,l_i]= [max(e_0,γ_i-h_i),min(l_0,y_i+h_i)]$ 


### Custom Data Loading

```python
class customized_vrp_loader(
  num_sample, 
  num_nodes, 
  mode,
  device,
  test_problem, 
  path
  )
```

**Bases:** `Dataset`

**Parameters:**
+ `num_sample`(`int`): number of samples in the dataset
+ `num_nodes`(`int`): number of customers
+ `mode`(`int`): different time window generation methods: when mode = 0, it is the generation method of MTPOMO; 
when mode = 1, it is the generation method of MVMoE.
+ `device`(`str`): device to store the data (CPU/GPU)
+ `test_problem`(`str`): problem_tpye,such as "CVRP","OVRP"...
+ `path`(`str`): the specified path of custom data

**Function:**
+ `__len__(self) -> int`: returns the total number of samples
+ `__getitem__(self, index) -> dict`: loads data from the specified `index`.

