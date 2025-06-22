# Exact Solvers
The traditional heuristic solvers currently include five exact solvers: LKH, HGS, EAX, Concorde, and OR-Tools.

## File introduction

+ `exact_solver_main.py:` Example usage, how to call a function
+ `param_setting.py:` Set parameters, such as LKH's "max_trials"
+ `solution.py:` Export the results of each solver in the format of (tour, cost)
+ `HGS_C.py` \ `LKH_CVRP.py` \ `LKH_TSP.py` \ `LKH_ATSP.py` \ `ortool_tsp.py` \ `ortool_cvrp.py ` \ `pyconcorde_run.py` \ `EAX.py` : Call interfaces for each solver, including auto-make function, call functions for data-loader, call functions for problem nodes

## Solvers intruduction and Required Settings
+ *LKH* : LKH (Lin-Kernighan-Helsgaun) is a high-performance heuristic algorithm specifically designed for solving the Traveling Salesman Problem (TSP) and its variants, employing dynamic λ-opt exchanges and candidate set strategies to obtain near-optimal solutions efficiently.
  + `MAX_TRIALS`(`int`): The number of iterations for LKH
  + `RUNS`(`int`): The number of times a problem is run
  + `TRACE_LEVEL`(`int`): Log information
  + `SEED`(`int`): Generate seeds for random numbers
+ *HGS* : HGS is a state-of-the-art VRP solver integrating genetic algorithms and adaptive large neighborhood search.
  + `time_threshold`(`int`): The running time of an instance solved by HGS
+ *EAX* : EAX is a genetic algorithm operator for TSP that performs edge recombination to escape local optima.
  + `population_size`(`int`): The count of candidate solutions maintained per generational iteration
  + `max_iterations`(`int`): The number of iterations for EAX 
+ *OR-Tools* :OR-Tools is Google's open-source optimization toolkit for constraint programming and heuristic search.
  + `num_vehicles`(`int`): only for CVRP, number of vehicles
+ *Concorde* : Concorde is the state-of-the-art exact TSP solver using branch-and-cut methods.
## Reference


|  Solver  |                                                                                   Paper Title                                                                                    |                   code                    |
|:--------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:-----------------------------------------:|
|   LKH    |           Helsgaun, K. (2000). An effective implementation of the Lin-Kernighan traveling salesman heuristic. European Journal of Operational Research, 126, 106-130.            | http://akira.ruc.dk/~keld/research/LKH-3/ |
|   EAX    | Nagata, Y., & Kobayashi, S. (2013). A Powerful Genetic Algorithm Using Edge Assembly Crossover for the Traveling Salesman Problem. INFORMS Journal on Computing, 25(2), 346-363. |  https://github.com/nagata-yuichi/GA-EAX  |
|   HGS    |              Vidal T. Hybrid genetic search for the CVRP: Open-source implementation and SWAP* neighborhood[J]. Computers & Operations Research, 2022, 140: 105643.              |    https://github.com/vidalt/HGS-CVRP     |
| OR-Tools |                                                       https://ai.googleblog.com/2019/09/or-tools-now-supports-integer.html                                                       |                     \                     |
| Concorde |    Applegate, David, Ribert Bixby, Vasek Chvatal, and William Cook. Concorde TSP solver. 2006.                                                                                                                                                                              |  https://github.com/jvkersch/pyconcorde   |


## Parameter Setting
In the `param_setting.py`, we classify all parameters to three types, solver setting, problem setting and working setting.
+ `--direct`: We offer two input formats: one allows direct input of node coordinates, while the other utilizes a `data_loader` format. The first format will be used when `--direct` is set to True.
```python
#cvrp for example
depot: The coordinates of the depot. [x_0, y_0]
loc: A list of node coordinates. [[x_1, y_1],[x_2, y_2],...,[x_n, y_n]]
demand: A list of node demands. [d_1, d_2, ..., d_n]
capacity: C(int)
```
+ `--save_as_txt`:We provide functionality to consolidate all information into a single file, with the final saved format structured as follows: 
```python
node_xy:xxx  
scale: xxx
distribution: xxx
attributes: xxx
solution: xxx
cost: xxx
time: xxx s  
LKH3/Concorde settings:
```
```python
depot_xy: xxx
node_xy:xxx  
node_demand: xxx
capacity: xxx
scale: xxx
distribution: xxx
attributes: xxx
solution: xxx
cost: xxx
time: xxx s  
LKH3/HGS settings:
```
## Start
**Using instruction**

- Referring to "exact_solver_main.py"
  - 1.Set the parameters in the file "param_setting.py"
  - 2.Call function "get_option()" in the file "param_setting.py"
  - 3.Choose to call a function based on the type of problem and solver type

**For example**

+ If you use HGS solver to solve a CVRP problem, and you use nodes directly rather than data_loader
```python
from exact_solver.param_setting import get_option
from exact_solver.HGS_C import hgs_solver

opts = get_option()
tour, cost = hgs_solver(depot, locs, demand, opts)
```
+ The results will be stored in "exact_solver/result_hgs", the executable will be stored in "exact_solver/use_exe/hgs"
------------------------------------------------------------------------------------------------------------------------    
+ Futhermore, if you use LKH solver to solve a TSP problem with multi-process, you should provide instances in data_loader format
```python
from exact_solver.param_setting import get_option
from exact_solver.LKH_TSP import lkh_solver_tsp_multiprocess

data_loader = TSPGenerator(data_size=opts.problem_number, problem_size=opts.problem_scale,
                                           batch_size=opts.batch_size, device=opts.device, path=None)
opts = get_option()
opts.cpus = 10
result = lkh_solver_tsp_multiprocess(data_loader, opts)
# result: [(tour_1, cost_1),(tour_2, cost_2),......(tour_n,cost_n)]
```
+ The results will be stored in "exact_solver/result_lkh_tsp", the executable will be stored in "exact_solver/use_exe/lkh"

## You should know
+ 1.Auto-make of all solvers are compatible with Linux system. We recommend using a Linux system.
+ 在 Linux 系统上安装 git
```bash
sudo apt-get update
sudo apt-get install git
```
+ 2.The following problems may occur in Auto-make of concorde: 
```python
ssl.sslCertverificationError: [SSL: CERTIFICATE VERIFY FALED] certificate verify failed: unable to get local issuer certificate ( ssl.c:135)
```
Now we need to add the following code at the beginning of 'setup. py' in PyConcorde:
```python
import ssl
ssl._create_default_https_context = ssl._create_unverified_context
```
+ 3.HGS solver in Windows system: Manually make rather than auto-make
  + (1)make: download mingw referring to 
    ```  
    https://sourceforge.net/projects/mingw
    ```  
  + (2)cmake: download cmake referring to
    ```
    https://cmake.org/download/
    ```
  + (3)Run the following command:
    ```bash
    mkdir build
    cd build
    cmake .. -DCMAKE_BUILD_TYPE=Release -G "Unix Makefiles"
    make bin
    ```
  Unlike linux, Windows will generate "hgs.exe" rather than "hgs"
