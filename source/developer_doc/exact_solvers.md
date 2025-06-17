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
  + `SEED`(`int`): Generate seeds for random numbers
+ *HGS* : HGS is a state-of-the-art VRP solver integrating genetic algorithms and adaptive large neighborhood search.
  + `time_threshold`(`int`): The running time of an instance solved by HGS
+ *EAX* : EAX is a genetic algorithm operator for TSP that performs edge recombination to escape local optima.
  + `population_size`(`int`): The count of candidate solutions maintained per generational iteration
  + `max_iterations`(`int`): The number of iterations for EAX 
+ *OR-Tools* :OR-Tools is Google's open-source optimization toolkit for constraint programming and heuristic search.
  + `num_vehicles`(`int`): only for CVRP, number of vehicles
+ *Concorde* : Concorde is the state-of-the-art exact TSP solver using branch-and-cut methods.
## Parameter Setting
In the param_setting.py, we classify all parameters to three characters, solver setting, problem setting and working setting.



## Start
**Using instruction**

- Referring to "exact_solver_main.py"
  - 1.Set the parameters in the file "param_setting.py"
  - 2.Call function "get_option()" in the file "param_setting.py"
  - 3.Choose to call a function based on the type of problem and solver type

**For example**

+ if you use HGS solver to solve a CVRP problem, and you use nodes directly rather than data_loader
```python
from exact_solver.param_setting import get_option
from exact_solver.HGS_C import hgs_solver

opts = get_option()
tour, cost = hgs_solver(depot, locs, demand, opts)
```
+ the results will be stored in "exact_solver/result_hgs", the executable will be stored in "exact_solver/use_exe/hgs"
    
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
## You should know
+ 1.Auto-make of all solvers are compatible with Linux system. We recommend using a Linux system.
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
    https://blog.csdn.net/qq_44940689/article/details/143415825?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522896da9b44ce2b68291305b8019dd4a46%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=896da9b44ce2b68291305b8019dd4a46&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-2-143415825-null-null.142^v101^pc_search_result_base2&utm_term=windows%20make&spm=1018.2226.3001.4187`
    ```  
  + (2)cmake: download cmake referring to
    ```
    https://blog.csdn.net/m0_67656158/article/details/143833925?ops_request_misc=&request_id=&biz_id=102&utm_term=windows%20cmake&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-143833925.142^v101^pc_search_result_base2&spm=1018.2226.3001.4187
    ```
  + (3)Run the following command:
    ```bash
    mkdir build
    cd build
    cmake .. -DCMAKE_BUILD_TYPE=Release -G "Unix Makefiles"
    make bin
    ```
  Unlike linux, Windows will generate "hgs.exe" rather than "hgs"
