# Why use a virtual environment
Developing a Python application, you may want to use modules that don’t ship with the standard library. Or sometimes, you need a specific version of a library for a bug to be fixed, or for some other reason.

It could be that application X needs version 1.0, but application Y needs version 2.0. This leaves one of them unable to run.  
To work with this, you create a virtual environment in Python. It enables multiple side-by-side installations of Python, one for each project. It doesn’t actually install separate copies of Python, but it does provide a clever way to keep different project environments isolated. 

# Using Virtual Environments
Since version 3.5, the use of `venv` is recommended for creating virtual environments. If you are using Python 3, then you should already have the `venv` module from the standard library installed.

**Steps:**

Start by making a new directory to work with:
```shell
$ mkdir python-virtual-environments && cd python-virtual-environments
```
