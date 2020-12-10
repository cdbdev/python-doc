# Why use a virtual environment
Developing a Python application, you may want to use modules that don’t ship with the standard library. Or sometimes, you need a specific version of a library for a bug to be fixed, or for some other reason.

It could be that application X needs version 1.0, but application Y needs version 2.0. This leaves one of them unable to run.  
To work with this, you create a virtual environment in Python. It enables multiple side-by-side installations of Python, one for each project. It doesn’t actually install separate copies of Python, but it does provide a clever way to keep different project environments isolated. 

# Using Virtual Environments
Since version 3.5, the use of `venv` is recommended for creating virtual environments. If you are using Python 3, then you should already have the `venv` module from the standard library installed.

Follow below steps to create and use a virtual environment:

Step 1: Start by making a new directory to work with
```shell
$ mkdir python-virtual-environments && cd python-virtual-environments
```
Step 2: Create a new virtual environment inside the directory
```shell
$ python3 -m venv my_env
```
Step 2a: Optionally inherit the system site packages
```shell
$ python3 -m venv my_venv --system-site-packages
```
Step 3: Activate the virtual environment  

To use a virtual environment, you activate and deactivate them.

```shell
$ my_venv/bin/activate
```
Once activated, you use python and pip like normal, except they will point to your virtual environment. When you activate a virtual environment, a few other things happen:

- python and pip in your PATH switches to the virtual environment Python
- Python will have access to the site packages from the virtual environment only
- pip packages and things installed with setup.py will go in your virtual environment's site packages
- Your PATH will be updated to include the Python virtual environment's script directory. Any executables installed through a package will be available in your shell path.

Step 4: Deactivate/exit virtual environment  
```shell
$ deactivate
```

Step 5: Optionally keep requirements  
To make your repo reusable, make sure to create a record of everything that’s installed in your new environment, run:
```shell
pip freeze > **requirements.txt**
```
If you are creating a new virtual environment from a requirements.txt file, you can run:  `pip install -f requirements.txt`. If you open your requirements file you will see a different package with its version in each line.


For more info, see: 

[https://realpython.com/python-virtual-environments-a-primer/](https://realpython.com/python-virtual-environments-a-primer/). 
[https://www.devdungeon.com/content/python-virtual-environments-tutorial](https://www.devdungeon.com/content/python-virtual-environments-tutorial)
