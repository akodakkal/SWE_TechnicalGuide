# Simulation_setup_and_run
This section provides the guidelines for solving a CFD prblem in KratosMultiphysics and is organized as follows.

1. Explanation of Project parameters for Kratos FluidDynamicsApplication
2. Modifications to Project parameters 
3. Remote Computing


## 1. Explanation of Project parameters for Kratos FluidDynamicsApplication

Project_parameters.json contains the all information that a Kratos FluidDynamicsApplication needs to read the model, solve a problem and export the output. It is directly read by the Main python file (which runs the simulation). The MDPA (model/mesh file) and fluid parameters file are linked to Main python file through Project_parameters.json. A project parameter file contains the following information. 

**1.analysis_stage** - Gives information to Kratos on which application to use

**2.problem_data** - It contains general settings for the simulation, such as the problem name, start and end time, etc. 

**3.processes** - The "processes" contains all the processes that needs to be executed during the simulation, such as boundary conditions, initial conditions, etc. For the project, we will use following processes - boundary_conditions_process_list, gravity and auxiliar_process_list. 

"boundary_conditions_process_list" a type of processes in Kratos which will be used to apply boundary conditions - inlet_velocity, outlet_pressure, slip condition and no slip condition to the model. Here make sure that the following BC settings applied from GiD are present. 

 - "apply_inlet_process" parameters for velocity input in rampup and runtime. 
 - "apply_outlet_process" for pressure settings applied
 - "apply_noslip_process" for the no slip conditions applied to the model
 - "apply_slip_process" for thr slip conditions applied to the model

4. **solver_settings** - This parameter gives the configuration for the solvers. It includes information on solver types, convergence tolerances, iteration limits, etc. The important parameter to check here is "time_step".

5. **output_processes** - The output_processes section defines the format (GiD, VTK, h5 and ASCII). Each format contains informaion on solution steps variables(such as pressure, velocity), model parts of which solution has to written, etc. 

**Note** GiD file formats are directly readable by GiD and VTK format is directly readable by Paraview. But h5 format needs to be converted into xdmf format for reading in ParaView. 


## 2. Modifications to Project parameters

The information provided below includes modifications that needs to be carried out in **ProjectParameters.json** file. In order to have a copy of your original ProjectParameters file, we will create copy of it and name them ProjectParametersCustom.json. Make sure to update the line in MainKratos.py referring to the project parameters from ProjectParameters &rarr; ProjectParametersCustom.

In ProjectParametersCustom "processes" and "output_processes" needs to be updated which is given below. It is also advised to refer to how the processes have been applied in the CFD_HighRiseExampleFine example. As different buildings may require additional process inputs, discuss with project coordinator. 

**Note that you are not restricted only to the following processes. You are free to add more outputs of your choice.**

### 2.1 output_processes

#### 2.1.1 HDF5 output process

Under "output_processes", GiD would have generated "gid_output" and "vtk_output". If it is present in your file, remove it. Instead the output files for our project is written in h5 format. It is because, our simulation has large domain size (high number of Finite elements) and requires many time steps. Hence VTK and GID file formats will occupy large memory when compared to h5 files. The files contains the pressure and velocity field in the fluid domain, and the pressure distribution in the surface of the structure. 
You can change the file naming convention and its directory in the `"file_name"` field. The parameters with `<>` inside `"file_name"` indicate a placeholder value and will change depending on what is written.
- `<model_part_name>` will change depending on the `"model_part_name"` chosen in the process.
- `<time>` will change depending on the simulation time that is currently printed by the process. 
- `<step>` will change depending on the time step that is currently printed by the process. You can use this as an alternative, but using `<time>` is of course more natural.

```json
{
    "output_processes"        : {
        "h5_output"            : [
            {
                "python_module": "single_mesh_temporal_output_process",
                "kratos_module": "KratosMultiphysics.HDF5Application",
                "Parameters": {
                    "model_part_name": "FluidModelPart.NoSlip3D_Structure",
                    "file_settings": {
                        "file_access_mode": "truncate",
                        "echo_level": 1,
                        "file_name": "results/hdf5_output/structure/<model_part_name>_T-<time>.h5",
                        "time_format": "0.2f"
                    },
                    "nodal_solution_step_data_settings": {
                        "list_of_variables": ["PRESSURE"]
                    },
                    "nodal_data_value_settings": {
                        "list_of_variables": ["SCALAR_MEAN", "SCALAR_VARIANCE"]
                    },
                    "output_time_settings": {
                        "time_frequency": 0.02,
                        "step_frequency": 1
                    }
                }
            },
            {
                "python_module": "single_mesh_temporal_output_process",
                "kratos_module": "KratosMultiphysics.HDF5Application",
                "Parameters": {
                    "model_part_name": "FluidModelPart",
                    "file_settings": {
                        "file_access_mode": "truncate",
                        "echo_level": 1,
                        "file_name": "results/hdf5_output/domain/<model_part_name>-<time>.h5",
                        "time_format": "0.2f"
                    },
                    "nodal_solution_step_data_settings": {
                        "list_of_variables": ["PRESSURE", "VELOCITY"]
                    },
                    "nodal_data_value_settings": {
                        "list_of_variables": ["SCALAR_MEAN", "SCALAR_VARIANCE",
                                              "VECTOR_3D_MEAN", "VECTOR_3D_VARIANCE"]
                    },
                    "output_time_settings": {
                        "time_frequency": 0.02,
                        "step_frequency": 1
                    }
                } 
            }
        ]
    }
}   
```

___
#### 2.1.2 Point output process

The `"point_output_process"` writes results from a chosen geometrical position (under "position") in the model to a file. It first searches the entity containing the requested output location and then interpolates the requested variable(s). The output can be requested for elements, conditions and nodes (under "entity_type"). If you choose "nodes", no geometrical interpolation is performed and the exact coordinates have to be specified, therefore it is advised to specify it for "elements". Output ranges from pressure to velocities in different directions (under "output_variables").

The `"multiple_points_output_process"` serves the same purpose as the `"point_output_process"`, however, here you can define several points of interest (under `"positions"`), instead of one.

For the project work, with the structure base positioned at (0,0,0) and structural height H, we are interested in the pressure and velocities around the point (-2H,0,H). Therefore, we should add a `"point_output_process"` with these coordinates in the ProjectParametersCustom file, similar to how it is done in the example. This point is important for the Pressure Coefficient calculation.

Below is an example of the json parameters for the `"point_output_process"`:
```json
{
    "output_processes"        : {
        "ascii_output"            : [
            {
                "python_module" : "point_output_process",
                "kratos_module" : "KratosMultiphysics",
                "process_name"  : "PointOutputProcess",
                "Parameters"    : {
                    "model_part_name"   : "FluidModelPart.fluid_computational_model_part",
                    "entity_type"       : "element",
                    "position"          : [-850.0, 0.0, 425.5],
                    "output_variables"  : ["PRESSURE", "VELOCITY_X", "VELOCITY_Y", "VELOCITY_Z"],
                    "output_file_settings"  : {
                        "file_name"  : "reference_point_output",
                        "output_path": "results/ascii_output/ref_point_minus2H_H",
                        "write_buffer_size": 1
                    }
                }
            }
        ]
    }
}
```
___
#### 2.1.3 Line output process

The `"line_output_process"` extracts output for several points along a line to a file. Internally it holds an object of type "MultiplePointsOutputProcess". By defining the "start_point" and "end_point" coordinates as well as the number of sampling points, we can receive multiple point outputs, without having to define each point separately.

For the project work, we are interested in different line outputs, such as:
- Around the contour of the building at h= 0.6H (midline) for pressure output P. Define the lines slightly outside the contour and not exactly on the surface to avoid numerical problems with location of the output points on the line. In case of a i.e. rectangular building cross sections, you will need to define 4 line output processes, with small distances ~0.1 m from the building contour.
- Pressure output in the centerline (front-top-back) in streamwise direction. In case the building is i.e. shaped as a cuboid, you will need to define 3 line output processes, with small distances ~0.1 m from the building contour.
- 3 lines, positioned respectively at -2H, -H and 0.5H, where we want to extract the velocities from the base to the height of 1.5H, in order to analyze the flow field

Below is an example of the json parameters for the `"line_output_process"`:
```json
{
    "output_processes"        : {
        "ascii_output"            : [
            {
                "python_module"   : "line_output_process",
                "kratos_module"   : "KratosMultiphysics",
                "process_name"    : "LineOutputProcess",
                "Parameters" : {
                    "model_part_name"   : "FluidModelPart.fluid_computational_model_part",
                    "start_point"       : [-1270.0, 0.0, 0.0],
                    "end_point"         : [-1270.0, 0.0, 650.0],
                    "sampling_points"   : 30,
                    "output_variables"  : ["PRESSURE", "VELOCITY_X", "VELOCITY_Y", "VELOCITY_Z"],
                    "output_file_settings": {            
                        "file_name"  : "line_output_at_minus3H",
                        "output_path": "results/ascii_output/line/line_output_at_minus3H/",
                        "write_buffer_size": 1
                    }
                }
            }
        ]
    }
}   
```

### 2.2 processes

Processes in coding aspect is Python module defined by a set of parameters that specify its behavior. These modules are built-in within Kratos. In addition new modules shall be written and integrated into Kratos during the execution of a simulation without the need for modifying the core Kratos source code. If a new process has been written and added to project parameter, these python modules must be placed in the folder where project parameter is present for kratos to read it during the simulation. For our project three additional process are needed. These process files can be found in the example. Copy the files to your folder.

#### 2.2.1 Force output process
After analyzing the flow field and the pressure around the building, we need to analyze the forces created from the wind loading on the building. There are 2 processes to analyze the forces on the building:

- `compute_global_force_process` is used to get the global forces (and moments) on the building in the flow- and body attached axis system. Here you need to define the reference point at the base of the building (0,0,0), in order to get the time series of the base forces and moments.
- `compute_level_force_process` is used to to get the level forces (and moment) in the flow- and body-attached axis system. We define the "start_point" and "end_point" coordinates from the building base (0,0,0) to the top of the building (0,0,H), as well as the number of intervals. The intervals are used as sampling points, meaning the number of intervals will represent the number of level forces you receive after the simulation. The forces at each level are then saved as a time history load, which will later be exported to ParOptBeam as a time history load for the computational structural dynamics (CSD), one way coupling (OWC).These processes are 

Below is an example of the json parameters for the `"compute_global_force_process"` and `"compute_level_force_process"`:
```json
{
    "processes"        : {
        "auxiliar_process_list"            : [
            {
                "python_module" : "compute_global_force_process",
                "process_name"  : "ComputeGlobalForceProcess",
                "Parameters"    : {
                    "model_part_name"       : "FluidModelPart.Drag_Structure",
                    "write_output_file"     : true,
                    "print_to_screen"       : false,
                    "reference_point"       : [0.0, 0.0, 0.0], 
                    "z_rotation_angle"      : 15.0,
                    "interval"              : [0.0, "End"],
                    "output_file_settings"  : {
                        "output_path"   : "results/ascii_output/forces",
                        "write_buffer_size": 1
                    }
                }
            },
            {
                "python_module" : "compute_level_force_process",
                "process_name"  : "ComputeLevelForceProcess",
                "Parameters"    : {
                    "model_part_name"       : "FluidModelPart.Drag_Structure",
                    "write_output_file"     : true,
                    "print_to_screen"       : false,
                    "start_point"           : [0.0, 0.0, 0.0],
                    "end_point"             : [0.0, 0.0, 425.5],
                    "z_rotation_angle"      : 15.0,
                    "intervals"             : 20,
                    "interval"              : [0.0, "End"],
                    "output_file_settings"  : {
                        "output_path"   : "results/ascii_output/forces/level_forces",
                        "write_buffer_size": 1
                    }
                }
            }
        ]
    }
}   
```
___
#### 2.2.2 CFL output process
The CFL number depends on the flow field, and differ from each case of simulation. Kratos has a module to print out the CFL number throughout the simulation. This process will give these following output for each time step:
- **max_value**: The maximum CFL number of the simulation.
- **max_id**: The ID of the element with the maximum CFL number.
- **mean**: Average CFL number in the simulation.
- **%>1.0**: Percentage of elements with CFL number above 1.0.
- **%>cfl_output_limit**: Percentage of elements with CFL number above the `"cfl_output_limit"`.

Below is an example of the json parameters for the `"cfl_output_process"`:
```json
{
    "processes"        : {
        "auxiliar_process_list"            : [{
            "kratos_module": "KratosMultiphysics.StatisticsApplication",
            "python_module": "spatial_statistics_process",
            "Parameters": {
                "model_part_name": "FluidModelPart.fluid_computational_model_part",
                "computation_processes": [
                    {
                        "kratos_module": "KratosMultiphysics.FluidDynamicsApplication",
                        "python_module": "compute_cfl_process",
                        "Parameters": {
                            "model_part_name": "FluidModelPart.fluid_computational_model_part",
                            "echo_level": 1
                        }
                    }
                ],
                "input_variable_settings": [
                    {
                        "norm_type": "magnitude",
                        "container": "element_non_historical",
                        "variable_names": [
                            "CFL_NUMBER"
                        ]
                    }
                ],
                "statistics_methods": [
                    {
                        "method_name": "variance"
                    },{
                        "method_name": "max"
                    },{
                        "method_name": "distribution",
                        "method_settings": {
                            "number_of_value_groups": 1,
                            "min_value": 1.0,
                            "max_value": 2.5
                        }
                    }
                ],
                "output_settings": {
                    "output_control_variable": "STEP",
                    "output_time_interval": 10, 
                    "write_kratos_version": true,
                    "write_time_stamp": true,
                    "output_value_precision": 2,
                    "output_value_length": 6,
                    "output_file_settings": {
                        "file_name": "cfl_results.dat",
                        "output_path": "results/ascii_output/",
                        "write_buffer_size": 1
                    }
                }
            }
        },{
            "kratos_module": "KratosMultiphysics.StatisticsApplication",
            "python_module": "temporal_statistics_process",
            "Parameters": {
                "model_part_name": "FluidModelPart.fluid_computational_model_part",
                "input_variable_settings": [
                    {
                        "method_name": "variance",
                        "norm_type": "none",
                        "container": "nodal_historical_non_historical",
                        "echo_level": 1,
                        "method_settings": {
                            "input_variables": [
                                "VELOCITY",
                                "PRESSURE"
                            ],
                            "output_mean_variables": [
                                "VECTOR_3D_MEAN",
                                "SCALAR_MEAN"
                            ],
                            "output_variance_variables": [
                                "VECTOR_3D_VARIANCE",
                                "SCALAR_VARIANCE"
                            ]
                        }
                    }
                ],
                "statistics_start_point_control_variable_name": "TIME",
                "statistics_start_point_control_value": 60
            }
        }
        ]
    }
} 
```

## 3.Remote Computing
Before continuing further with the remote computing, make sure the following has been done:
- Have the necessary tools for Remote Computing installed ([eduVPN](https://github.com/akodakkal/SWE_TechnicalGuide/blob/main/pages/Kratos/Workshops/Hitchhiker_Guide_SWE/Installation%20Guides/Remote_Computing.md#vpn) and [Git Bash](https://github.com/akodakkal/SWE_TechnicalGuide/blob/main/pages/Kratos/Workshops/Hitchhiker_Guide_SWE/Installation%20Guides/Remote_Computing.md#secure-shell-ssh-client)).
- Check if the GiD Model of the building as well as the wind characteristics are correct.
- Check if the simulation [parameters](Preprocessing.md#3-simulation-parameters) and [processes](Preprocessing.md#2-processes) of the ProjectParametersCustom.json and MainKratosCustom.py are adjusted.
- Test if the run begins (3-5 time steps, (with all possible necessary output turned on and additional processes)).

Now you are prepared for the full simulation on the chairs cluster.

___
### 3.1 Useful Linux Commands
As a start, it is useful to get to know a couple of linux commands. The angle brackets "< >" are used to indicate a placeholder value, which you should replace with your own input.

| Command | Function | 
| -------- | -------- |
| pwd | Shows the current working directory. | 
| cd `<path>` | Goes to the selected path. | 
| cd .. | Goes back to the parent folder. | 
| ls | Lists all the files and folders that are at the current location | 
| mv `<file-name>` `<new-path>` | Moves a file to a certain location. It is also used to change the name of a file. | 
| mv -r `<folder-name>` `<new-path>` | The "-r" option means "recursively". Moves the folder and all inside to a certain location. | 
| cp `<file-name>` `<new-name>` | Copies a file. | 
| cp -r `<file-name>` `<new-name>` | The "-r" option means "recursively". Copies a folder and all inside it. | 
| vim | Visualizes a text file. Pressing "I" you enter in "insert mode" to edit it. To exit press "Esc" and the ":w" to save it and ":q" to quit. If the name of the file does not exist it creates a new one. | 
| mkdir | Creates a directory | 
| htop | Checks the usage of processors and RAM | 
| tail -n `<num-lines>` `<file-name>` | Prints in the screen the last <num-lines> rows of a text file. Very useful to check if the simulation is running. | 
| sh | Execute an .sh file | 

___
### 3.2 Login to cluster
 
You will need to initially be connected to the university network to find the computers. For that, use EduVPN.

The Credentials for the CIP-Pool computer (normal computer) and the Cluster will be given during the course. You will need the following:

- User name
- IP-address
- Password

The login is done directly through the "ssh" command in the Git Bash terminal:

```shell
$ ssh -oHostKeyAlgorithms=+ssh-dss <user-name>@<IP-address>
```

**Note: Some operating systems might have problems connecting via ssh to the cluster, due to the old operating system at the cluster, that is why we use the parameter "-oHostKeyAlgorithms=+ssh-dss". If you face any error using this command try to substitute it for "-oHostKeyAlgorithms=+ssh-rsa"*

___
### 3.3 Transfer files
  
The file structure in the cluster is divided in the following folders:
- Software:
  - Kratos &rarr; Folder where Kratos is compiled. **You should not touch it.**
  - setup_kratos.sh &rarr; File to add Kratos to the path. **You should not touch it.**
- Documents:
  - Tests &rarr; Folder with some examples and tests.
  - Templates &rarr; Here are saved some important files you will need to copy.
  - Group`<group-data>` &rarr; Multiple folders, each for one group. Here you can save all your documents and simulations.
  
The next step is to transfer your simulation file to your group folder in the cluster. To transfer files from your personal computer to the remote computers (or the other way around) we use the secure copy ("scp") command:
  
- To copy from your personal computer to the remote location, type in your terminal (mind the spaces):
  ```shell  
  $ scp -r -oHostKeyAlgorithms=+ssh-dss <folder-to-copy> <user-name>@<IP-address>:<path>
  ```

- To copy from the statik computer to the remote location, type in your terminal:
  ```shell
  $ scp -r -oHostKeyAlgorithms=+ssh-dss <user-name>@<IP-address>:<path-folder-to-copy> <paste-destination>
  ```
  
Remember to put the parameter `-r` so the folder and all the contents can be copied. If you only want a file, omit the parameter `-r`. 

___
### 3.4 Running in the cluster

First, here are some useful commands for the cluster:

| Command | Function | 
| -------- | -------- |
| qstat | Shows own submitted jobs. | 
| qdel `<job-id>` | Cancel a job | 
| qstat -f | Show the state of all the nodes in the computer, including where each job is running.| 

Let us imagine, that you want to run a script (e.g. MainKratosCustom.py) located in a certain folder of the cluster. These would be the steps to launch the job with the qsub command:

- Log in on the head of the Statik cluster (see instructions above).
- Navigate to the folder where you have the script you want to run:
```shell
$ cd <path-to-simulation-folder>
```
- Using the template you have in `../Documents/Templates/q_run.sh` create a **q_run.sh** file and save it in the same folder than your **MainKratosCustom.py**. This file is in charge of running the simulation. **Please specify your** ***\<job-name\>*** **(it must start with a "G" followed by the number of your group, and preferably something below 8 characters total)**. You can also change the error (after -e) and output (after -o) file names, which will serve to output any errors that might happen during the simulation and the standard prompt output respectively. By last, write the command that you want to execute. In your case: 
```shell
$ mpirun -x LD_LIBRARY_PATH --bind-to core --map-by socket python3 MainKratosCustom.py
```
The simulation will be using MPI parallel processing feature, which speeds up your simulation. Please make sure you change the `"parallel_type"` from `"OpenMp"` to `"MPI"` in "ProjectParametersCustom.json" as:
```json
{
    "problem_data"     : {
        "parallel_type" : "MPI"
    }
}
```

- Using the template you have in `../Documents/Templates/q_submit.sh` create a **q_submit.sh** file and save it in the same folder than your **MainKratosCustom.py**. This file helps you launch the job, calling the **q_run.sh** file. To do it, just execute it:
```shell
$ sh q_submit.sh
```

- As an alternative to the previous step, one can also run the command to launch the job directly in the terminal, so the **q_submit.sh** file is not needed:
```shell
$ qsub -pe impi_tight_fu <num-of-cores> -V q_run.sh
```

- Check that the simulation is running with the **qstat** or **qstat -f** commands. If it is running, go to one of the output files and use the following command to visualize the last lines and check that they are being written:
```shell
$ tail -n <number-of-lines-to-print> <output-file-name>
```   

When the simulation ends, the nodes should be liberated automatically. You can then [copy the results back to your computer](#3-transfer-files), the same way you copied the results from your computer to the cluster, just the other way around. The next step is the [postprocessing of the simulation results](Postprocessing.md).


