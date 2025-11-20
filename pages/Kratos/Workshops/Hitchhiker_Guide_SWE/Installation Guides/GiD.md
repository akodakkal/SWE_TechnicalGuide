# GiD and Kratos Problem Type
GiD and the Kratos problem type for GiD will be used during the project work for the CFD simulation of the structure. GiD supports the geometrical modelling of the structure, meshing, boundary conditions, system properties, etc. Kratos is used as a solver, from which we receive our results. It is a multi-physics simulation code. Aside from CFD, GiD + Kratos might also be used for other purposes, such as Structural/Dynamic Analysis, Fluid-Structure-Interaction etc. Kratos problem type in GiD add some features to support Kratos in GiD, which will generate the input for the solver.

## Installation
### 1. Install GiD:
[Download GiD at their website](https://www.gidsimulation.com/gid-for-science/downloads/), choose your operating system and download *“GiD 17.0.5 64 bits"*. Then click on downloaded the executable to install it.

![GiD_Download](../../../../../images/WindEngineering/GiD_Download.png)

In order to use GiD, you will need to activate a professional license, as the free license is limited. You will be provided with access to this license during the course. After registering, open GiD and go to **" Help &rarr; Register GiD &rarr; Named user &rarr; sign in"**. To sign in, put your TUM email address and password you used during registration.

### 2. Install Kratos Problem Type: 
[Download Kratos problem type](https://www.gidsimulation.com/gid-for-science/downloads/), version *“kratos- 9.5.1"* as a zip file. 

![Kratos_problem_type_Download](../../../../../images/WindEngineering/Kratos_problem_type_download.png)

Extract kratos.gid folder(inside kratos- 9.5.1) to ../images/GiD/GiD 17.0.5/problemtypes. 

![Kratos_problem_type_Extraction](../../../../../images/WindEngineering/Kratos_problem_type_extraction.png)

Restart GiD and go to Data → Problem type. Kratos should appear. If Kratos is not found, then it is not properly extracted in the location. 

![GiD_Kratos_problem_type](../../../../../images/WindEngineering/GiD_Kratos_problem_type.png)

Follow [this example](Testing_Kratos_Installation.md) to see whether you have properly installed GiD and Kratos
