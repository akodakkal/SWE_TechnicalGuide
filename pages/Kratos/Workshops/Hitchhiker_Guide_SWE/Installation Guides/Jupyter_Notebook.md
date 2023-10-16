---
title: Jupyter Notebook
keywords: 
tags: [Jupyter_Notebook.md]
sidebar: kratos_workshops
summary: 
---
# Jupyter Notebook
Jupyter Notebook is a web-based interactive computing platform. Its biggest advantage is its interactive block output, making it easy for the user to understand certain processes in the code easier. 

## 1. Getting started with Jupyter Notebook
Jupyter Notebook is installed together with the anaconda package. In case it is not installed due to reasons, you can write and run **"pip install notebook"** in the terminal. 

After installing Jupyter Notebook, you can open it either from the Anaconda Navigator, or directly from the start menu **"Start &rarr; Jupyter Notebook"**. In Linux you need to navigate to the required folder in the terminal window and then type **"jupyter notebook"**. This is also applicable in windows. The drive of which the jupyter notebook will operate depends on the directory it is launched from the terminal. 

## 2. Adding Jupyter shortcut to the desired directory
In order to add the Jupyter shortcut to the desired directory, you can open the file location **"Start &rarr; right-click Jupyter Notebook &rarr; Open file location"**. When in the file location, go to **"right-click Jupyter Notebook Shortcut &rarr; Properties"**. Under the properties you can go **to "Shortcut &rarr; Target"**. Replace **%USERPROFILE%** with the desired directory (```D:\..```). Now you have a shortcut of Jupyter Notebook in your desired directory.

## 3. Jupyter Notebook Interface
The Jupyter Notebook Interface consists of the **Code Cell**, where the code is written and will run, and the **Markdown Cell** in which comments and explanations are written in markdown, which is a simple markup language. 

You can run the full script by going to **"Kernel &rarr; Restart & Run All"**. In case you want to run only one block of script, left-click to select the script, then click on **"Run"**. 

## 4. Keyboard Shortcuts for Jupyter Notebook
For an efficient way of working, some keyboard shortcuts are presented:

To enter the **COMMAND** mode press **ESC** or click anywhere outside the cell. You will see grey border around the cell with blue left margin. When you are in Command mode, you can edit your notebook but you can't type in the cells.

Once in command mode, you can (**case sensitive**):

| Command | Function |
| ------- | ---------|
| **&uarr;**/**&darr;** | Scroll up or down your cells | 
| **A** / **B** | Insert a new cell above or below the active cell | 
| **M** | Transform the active cell to a Markdown cell | 
| **Y** | Set the active cell to a code cell | 
| **D + D** | Delete the active cell | 
| **Z** | Undo cell deletion | 
| **Shift + &uarr;**/**&darr;** | Select multiple cells at once | 
| **Shift + M** | Merge the selected cells | 
| **Ctrl + Shift + -** | In edit mode, split the active cell at the cursor | 

You can also click and Shift + Click in the margin to the left of your cells to select them.

For more keyboard shortcuts, go to **Help &rarr; Keyboard Shortcuts**

## 5. Using Jupyter in the browser

Alternatively, you can [open Jupyter in the browser](https://jupyter.org/try-jupyter/lab?path=notebooks%2FIntro.ipynb){:target="_blank"} without having to install the application. Using the web app, you can upload your Jupyter Notebooks (.ipynb files) and then view, edit and run your scripts via the browser. 
