# Python and Kratos Installation
Most of the tools necessary for the course and project require to use python. The version necessary for the course is **Python 3.12**. We recommend installing it through the Python Store, which will be explained in the following guide. Alternatively you can use Anaconda, although we prefer the following way for simplicity.

## **1. Python 3.12 Installation**
Go to the Python Store and install Python 3.12 on your computer.

![python_3_12](../../../../../images/WindEngineering/python_3_12.PNG)

You can check whether Python has been successfully installed by using  “python” command in the command prompt. You should get the following output when you have successfully installed Python:
![python_3_12_output](../../../../../images/WindEngineering/python_3_12_output.PNG)

Use the `exit()` command to exit from the Python interpreter.


## **2. Python Packages Installation**
Install following Python packages using the commands below:

```console
python3.12 -m pip install scipy
python3.12 -m pip install matplotlib
python3.12 -m pip install ffmpeg
```

## **3. Enable the “long paths” handling**
Run a PowerShell administrator terminal (Windows)

```console
Start-Process powershell -Verb runAs
```

and run the following two lines of code:

```console
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
```

You should get the following output:

![long_paths_handling](../../../../../images/WindEngineering/long_paths_handling.png)

## **4. Kratos Multiphysics Installation**
Kratos is installed as a Python package. It is important to install version 9.5.1. For that, you can run the following command:

```console
python3.12 -m pip install --upgrade --force-reinstall --no-cache-dir KratosMultiphysics==9.5.1
python3.12 -m pip install --upgrade --force-reinstall --no-cache-dir KratosFluidDynamicsApplication==9.5.1
python3.12 -m pip install --upgrade --force-reinstall --no-cache-dir KratosHDF5Application==9.5.1
python3.12 -m pip install --upgrade --force-reinstall --no-cache-dir KratosStatisticsApplication==9.5.1
```
## **5. H5py**
'''console 
python3.12 -m pip install --upgrade --force-reinstall --no-cache-dir h5py
'''
