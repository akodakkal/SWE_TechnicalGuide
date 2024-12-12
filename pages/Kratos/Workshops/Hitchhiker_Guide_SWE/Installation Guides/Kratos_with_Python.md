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
## **5. Solution to problems with KratosHDF5Application H5py**
The version 9.5.1 of HDF5 application has a missing dynamic link library (DLL), which needs to be solved manually. If you don't know what a DLL is, don't worry, you can just follow this steps to make the HDF5 application work.

First, we can install an extra Python package which has our missing library using the following command:

```console 
python3.12 -m pip install --upgrade --force-reinstall --no-cache-dir h5py
```

Then, we need to find where did python install the new package. You can do it by running this command:

```console 
python3.12 -m pip show h5py
```

Under "Location:" you can find the path to all python packages. You can open then the "h5py" folder, inside it locate the file "hdf5.dll" and copy it. Then, go back to the path as before and enter this time in the folder "KratosMultiphysics". Go to the folder ".lib" and paste the "hdf5.dll" file there.

Everything is ready to go! you can test it by running a python file with the following lines:

```console 
import KratosMultiphysics
import KratosMultiphysics.FluidDynamicsApplication
import KratosMultiphysics.StatisticsApplication
import KratosMultiphysics.HDF5Application
```



