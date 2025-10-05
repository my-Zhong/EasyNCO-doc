# Quickstart
The platform provides two main ways of usage to meet the needs of different types of users.

## 1. Graphical User Interface (GUI) Mode

### Features
+ Intuitive and user-friendly interface.
+ Supports operations through menus, buttons, and configuration panel, without requiring code.
+ Integrated workflows for common tasks such as **data upload**, **configuration management**, and **process monitoring**.
+ Suitable for **non-technical users** or **first-time users of the platform**.

### Usage
#### Step1: Run GUI
1. Place the `EasyCO` project folder in the proper directory on your remote server.
2. Execute the GUI's Python script `run_gui_test.py` on your local machine.

#### Step2: Configure in GUI
1. Input the server-related information, such as **host**, **port**, and **username**, select the **login type**, and click the Connect button to connect to the server.
2. Select a **Problem** from the list.
3. Select a **Solver** from the list.
4. Set the **Parameter** of the solver.
5. Modify **Test Parameter** or **Training Parameter** carefully.

#### Step3: click the button of Start
1. Click `start` button to start process.
2. Get the **Log File Output** and **Summary Output**.
3. Click `stop` process to terminate the process, or wait until it finishes.


## 2. Command Line Interface Mode

### Features

+ Operates by entering commands in the terminal to run tasks or execute scripts.
+ Provides extensive command options and arguments for customization.
+ Can be seamlessly integrated with existing development toolchains and automation scripts.
+ Suitable for **developers, engineers, or advanced users**.

### Usage

#### Step1: Basic configuration
Define the fundamental settings—such as **problem type**, **problem size**, **dataset volume**, **solver method** and so on—in the root configuration file `config.yaml`.

+ This file controls the overall behavior of the platform.
+ Update these values before starting training or evaluation to match your specific experiment or dataset.

#### Step2: Solver-specific parameters
Each solver can have its own fine-tuning options.
+ To modify these, open the corresponding `YAML` file inside the `settings` directory.
+ For example, you might adjust **learning rates**, **iteration limits**, or other algorithm-specific hyperparameters here.

#### Step3: Running experiments
After configuration is complete, execute the appropriate Python script from the project root:

+ Model testing: `python eval.py` will load the specified configuration and run the evaluation pipeline.
+ Model training: `python train.py` will launch the full training workflow and save model checkpoints according to the settings

💡Note: If you prefer not to modify the parameters in the `YAML file`, you can also set them directly from the command line, for example:
```python
python train.py \
    settings=... \
    mode=... \
    problem=...\
    settings.model.{...}=... \
    settings.model.{...}=... \
    settings.model.{...}=... \
```
> Command-line examples for different methods can be found in the methods introduction section.