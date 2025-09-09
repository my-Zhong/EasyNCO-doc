Graphical User Interface (GUI)
================================

>Welcome to the GUI. This interface is designed to provide you with a convenient and straightforward user experience. Simply configure your user information and parameters within the GUI, and it will return the results for you. Please see the user manual below for detailed instructions.


## Main Component
> To run the GUI correctly, you must place the "EasyCO" project folder in the proper directory on your remote server. Then, execute the GUI's Python script `run_gui_test.py` on your local machine (an operating system that can render a graphical interface, typically Windows).


- **Menu Bar**
insert picture here
    + `github`: Redirects to the `EasyCO` GitHub repository.
    + `documention`:  Redirects to the `EasyCO` documentation page for user guides and references.

- **Server Setup**
insert picture here
    + `host`: The server’s IP address or domain name to connect to.
    + `port`: The specific port number through which the connection is established.
    + `username`: The user identity for logging into the server.
    + `login method`: The authentication method, either key-based or password-based.
    + `key`: The path to the private key file used for secure login (only if key-based method is chosen).
    + `device`: The target computation device on the server (e.g., CPU, GPU such as cuda:0).
    + `connect`: The button to initiate the connection to the server with the provided settings

- **Problem**
insert picture here
    + `Problem`: click the problem you want to solve in the `Scroll Bar` Frame.
    + `scale`: If you tick the box on the left, it means you enable the *vary scale* function. Enter the range correctly(min-max).  In this case, a dataset containing multiple scales will be provided to the solver, and the solver will filter out the instances within the specified range and solve them.
    + `capacity`:  If you check the box on the left, it enables the *vary capacity* function. Enter the range correctly(min-max). different capacity values will be provided to the solver, and the solver will select the instances satisfiy the specified range and solve them.


- **Solver**
> 💡**Tips**:  In case that `Solver` shows properly, please select `Problem` first.
insert picutre here
    + `Solver`: Click the solver you want to apply for the problem in the `Scroll Bar` Frame. Please select `problem` before you select `solver`. 
    + `Save`: Click `Save` button to view historical test results. A pop-up will appear where you can select the results you wish to visualize. The system will automatically create a local backup of the log files (located in the `GUI/log_temp/` directory) for your convenience.  
    insert picture here: tick the left frame that you want.
    + `Plot`: Click `Plot` button to see the visualized results.
    insert picture here: In the `Select Metric` dropdown, you can choose the metric you want to visualize: `Score`, `Augmented Score`, `Gap`, or `Augmented Gap`. You can also customize the plotting parameters in the `Plot Parameters` sidebar. 


- **Parameter**
> 💡**Tips**:  In case that `Parameter` shows properly, please select `Problem` and `Solver` first.
insert picutre here
    + `Parameter`:  The `settings` for specific `Solver` would be shown in this frame, at least you choose `Problem` and `Solver` properly.
    insert picture here: you can modified settings_parameters for `Solver` directly in GUI. 

- **Test Parameter**

- **Log File Output**

- **Summary Output**



## Instructions