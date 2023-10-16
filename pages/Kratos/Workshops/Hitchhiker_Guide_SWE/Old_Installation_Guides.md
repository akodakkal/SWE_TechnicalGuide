---
title: Installation Guides
keywords: 
tags: [Installation_Guides.md]
sidebar: kratos_workshops
summary: 
---
# Installation Guides
The following part of the guide will show you all the necessary tools to be prepared for the Structural Wind Engineering (SWE) course and the project work. The guide contains the necessary steps for the installation of the tools and a couple of tips to get started. It is advised to go through all the installation steps, without neglecting any parts, in order to avoid potential errors when using the tools during the course.

It is important to note that, given the annual organization of the Structural Wind Engineering (SWE) course, the mandatory program versions may be subject to modification. As such, participants will be clarified regarding the specific versions required during the course.

___
## 1. Python
Most of the tools necessary for the course and project require to use python. The version necessary for the course is Python 3.10, together with the following modules:
- Numpy
- Sympy
- Matplotlib
- SciPy
- etc.

An Integrated Development Environment (**IDE**) is highly recommended to carry on the Python processes. The project course will mostly focus on using **Visual Studio Code**, however, other IDE-s such as **Spyder** or **PyCharm** are also available.

Another python-based program you will need is **Jupyter Notebook**.
During the semester you will be supplied with Jupyter Notebook Scripts, which are helpful to consolidate your knowledge but also to use as helpful tools for the project work. 

___
### 1.1. Anaconda 

An efficient way to install Python together with all its modules is installing the latest **Anaconda package** (Anaconda 22.10), as it comes with some necessary modules and a web application of **Jupyter Notebook**. Visual Studio Code can also be installed together with Anaconda. 

Anaconda is available for [download under this page](https://www.anaconda.com/distribution/){:target="_blank"}. It is available for Windows, Linux, Mac OS. You can choose the Operating System of your choice by clicking on its respective symbol.

Choose the Python 3.10 version with 64-bit, if possible. After the download is finished, run the .exe file and go through each step of the setup. Typically, your install location should be in your "Users" file in the local disk. 

Example: `C:\Users\maxmustermann\anaconda3`

___
### 1.5. More useful links for python (optional)

#### 1. Windows
- [WinPython](https://winpython.github.io/){:target="_blank"} comes together with Spyder as IDE.
- [Notepad++](https://notepad-plus-plus.org/){:target="_blank"}, Lightweight text editor for editing text files.
- [PyCharm](https://www.jetbrains.com/pycharm/){:target="_blank"}, another IDE, just like Spyder or VS Code.
- [Eclipse](https://www.eclipse.org/){:target="_blank"} together with the [Python extension](http://www.pydev.org/){:target="_blank"}.

#### 2. Linux
The sources for the installation of Python can be found here:
 - [Python](https://www.python.org/downloads/source/){:target="_blank"}
 - [Sci-Py packages](https://www.scipy.org/install.html){:target="_blank"}

___
## 2. Installation Guide for GiD & Kratos module
GiD and the Kratos module for GiD will be used during the project work for the CFD simulation of the structure. GiD supports the geometrical modelling of the structure, meshing, boundary conditions, system properties, etc. Kratos is used as a solver, from which we receive our results. It is a multi-physics simulation code. Aside from CFD, GiD + Kratos might also be used for other purposes, such as Structural/Dynamic Analysis, Fluid-Structure-Interaction etc. Kratos module in GiD add some features to support Kratos in GiD, which will generate the input for the solver.

___
### 2.1. Download and Install GiD

[Download GiD at their website](https://www.gidsimulation.com/gid-for-science/downloads/){:target="_blank"}. Choose your operating system **(Windows, Linux, Mac OS)**. For Mac, only GiD is available, without the precompiled Kratos problemtype. 

Download the recommended version during the course. The scripts of the course are frequently tested with the newest GiD versions. In this case you are recommended to use GiD 16.1.3.

**For Windows** &rarr; Download the file and go through the installation. 

**For Linux** &rarr; Install using the graphical way or through console:
- Enable executable: chmod +x gid16.0.1-linux-x64-Install, then double click to use the graphical install.

**or**
- ./gid16.0.1-linux-x64-Install --mode console.

In order to use GiD, you will need to activate a professional license, as the free license is limited. You will be provided with access to this license during the course.

After registering, open GiD and go to **" Help &rarr; Register GiD &rarr; Named user &rarr; sign in"**. To sign in, put your TUM email adress and password you used during registration.

___
### 2.2. Download and Install Kratos module

#### 1. Option: Install Kratos module from GiD
You can install Kratos module directly from GiD, by going to **"Menu bar &rarr; Data &rarr; Problemtype &rarr; Internet Retrieve"**. A window will then open. In the offered modules, select **Kratos (9.2.2) &rarr; Retrieve Module**. 

#### 2. Option: Install Kratos module from GiD archive files
You can also install [Kratos module](https://www.gidsimulation.com/downloads/kratos/){:target="_blank"} manually from [GiD archive files.](https://downloads.gidsimulation.com/#gidmodules/){:target="_blank"}. Download the latest version of Kratos (9.2.2) for your respective operating system. The x64 version is recommended as further exercises have been created and tested using this one. After downloading the folder, unzip it and move the **"kratos.gid"** folder into the **“problemtypes“** folder inside the installation directory of GiD (overwriting the existing folder). By default, the installation directory is:

```C:\Program Files\GiD\GiD 16.1.3\problemtypes```

The next time GiD is started, the kratos problemtype can be found under **Data &rarr; Problem type &rarr; kratos**.

