# Python Interpreter

The interpreter is the program you’ll need to run Python code and scripts. Technically, the interpreter is a layer of software that works between your program and your computer hardware to get your code running.

Depending on the Python implementation you use, the interpreter can be:
- A program written in C, like CPython, which is the core implementation of the language
- A program written in Java, like Jython
- A program written in Python itself, like PyPy
- A program implemented in .NET, like IronPython

Whatever form the interpreter takes, the code you write will always be run by this program. 

The interpreter is able to run Python code in two different ways:
- As a script or module
- As a piece of code typed into an interactive session

# Run Python Code Interactively

A widely used way to run Python code is through an interactive session. To start a Python interactive session, just open a command-line or terminal and then type in python, or python3 depending on your Python installation, and then hit <ENTER>.

Here’s an example of how to do this on Linux:

```bash
$ python3
Python 3.6.7 (default, Oct 22 2018, 11:32:17) [GCC 8.2.0] on linux Type "help", "copyright", "credits" or "license" for more information. >>>
```

The standard prompt for the interactive mode is >>>, so as soon as you see these characters, you’ll know you are in.

Now, you can write and run Python code as you wish, with the only drawback being that when you close the session, your code will be gone.

```python
>>> print('Hello World!')
Hello World!
>>> 2 + 5
7
>>> print('Welcome to Real Python!')
Welcome to Real Python!
```

An interactive session will allow you to test every piece of code you write, which makes it an awesome development tool and an excellent place to experiment with the language and test Python code on the fly.

To exit interactive mode, you can use the following: `quit()` or `exit()`, which are built-in functions.

# How Does the Interpreter Run Python Scripts?
When you try to run Python scripts, a multi-step process begins. In this process the interpreter will:
1. **Process the statements of your script in a sequential fashion**
2. **Compile the source code to an intermediate format known as bytecode**  This bytecode is a translation of the code into a lower-level language that’s platform-independent. Its purpose is to optimize code execution. So, the next time the interpreter runs your code, it’ll bypass this compilation step. Strictly speaking, this code optimization is only for modules (imported files), not for executable scripts.  

3. **Ship off the code for execution**  At this point, something known as a Python Virtual Machine (PVM) comes into action. The PVM is the runtime engine of Python. It is a cycle that iterates over the instructions of your bytecode to run them one by one.

The PVM is not an isolated component of Python. It’s just part of the Python system you’ve installed on your machine. Technically, the PVM is the last step of what is called the Python interpreter.

The whole process to run Python scripts is known as the **Python Execution Model**.

> **Note**: This description of the Python Execution Model corresponds to the core implementation of the language, that is, CPython. As this is not a language requirement, it may be subject to future changes.

# Run Python Scripts Using the Command-Line
A Python interactive session will allow you to write a lot of lines of code, but once you close the session, you lose everything you’ve written. That’s why the usual way of writing Python programs is by using plain text files. By convention, those files will use the .py extension. (On Windows systems the extension can also be .pyw.)

## Using the python Command
To run Python scripts with the python command, you need to open a command-line and type in the word python, or python3 if you have both versions, followed by the path to your script, just like this:

```bash
$ python3 hello.py
Hello World!
```

This is the most basic and practical way to run Python scripts.

## Running Modules With the -m Option
Python offers a series of command-line options that you can use according to your needs. For example, if you want to run a Python module, you can use the command `python -m <module-name>`.

The `-m` option searches `sys.path` for the module name and runs its content as `__main__`:

```bash
$ python3 -m hello
Hello World!
```
This option is especially useful if you want to run a module that depends on another module (module path).  
Example:
```bash
modtest-git/
├── modtest
│   ├── sub1
│   │   ├── mod1.py
│   ├── sub2
│   │   ├── mod2.py
```
Code of `mod1.py`:
```python
import sub2.mod2 as mod2

def print_hello():
	print(mod2.rev("ih"))
    
if __name__ == '__main__':
    print_hello()
```
Code of `mod2.py`:
```python
def rev(str):
	return str[::-1]
```
Here if you want to run `mod1.py` (which depends on `mod2.py`), you have to run the following from within the 'modtest' directory:
```bash
python -m sub1.mod1
```
If we were to run: `python mod1.py`, we would get the following error:
```bash
Traceback (most recent call last):
  File "mod1.py", line 1, in <module>
    import sub2.mod2 as mod2
ModuleNotFoundError: No module named 'sub2'
```

## Using the Script Filename
On recent versions of Windows, it is possible to run Python scripts by simply entering the name of the file containing the code at the command prompt:

```bash
C:\devspace> hello.py
Hello World!
```

This is possible because Windows uses the system registry and the file association to determine which program to use for running a particular file.

On Unix-like systems, such as GNU/Linux, you can achieve something similar. You’ll only have to add a first line with the text #!/usr/bin/env python, just as you did with hello.py.

For Python, this is a simple comment, but for the operating system, this line indicates what program must be used to run the file.

This line begins with the #! character combination, which is commonly called **hash bang** or **shebang**, and continues with the path to the interpreter.

There are two ways to specify the path to the interpreter:

- **#!/usr/bin/python**: writing the absolute path
- **#!/usr/bin/env python**: using the operating system env command, which locates and executes Python by searching the PATH environment variable

This last option is useful if you bear in mind that not all Unix-like systems locate the interpreter in the same place.

## Run Python Scripts Interactively
It is also possible to run Python scripts and modules from an interactive session. This option offers you a variety of possibilities.

### Taking Advantage of import
_When you import a module_, what really happens is that you load its contents for later access and use. The interesting thing about this process is that import runs the code as its final step.

When the module contains only classes, functions, variables, and constants definitions, you probably won’t be aware that the code was actually run, but when the module includes calls to functions, methods, or other statements that generate visible results, then you’ll witness its execution.

This provides you with another option to run Python scripts:

```python
>>> import hello
Hello World!
```

You’ll have to note that this option works only once per session. After the first import, successive import executions do nothing, even if you modify the content of the module. This is because import operations are expensive and therefore run only once. Here’s an example:

```python
>>> import hello  # Do nothing
>>> import hello  # Do nothing again
```

These two import operations do nothing, because Python knows that hello has already been imported.

There are some requirements for this method to work:

- The file with the Python code must be located in your current working directory.
- The file must be in the **Python Module Search Path (PMSP)**, where Python looks for the modules and packages you import.
To know what’s in your current PMSP, you can run the following code:

```python
>>> import sys
>>> for path in sys.path:
...     print(path)
...
/usr/lib/python36.zip
/usr/lib/python3.6
/usr/lib/python3.6/lib-dynload
/usr/local/lib/python3.6/dist-packages
/usr/lib/python3/dist-packages
```

Running this code, you’ll get the list of directories and .zip files where Python searches the modules you import.

### Using importlib and imp
In the **Python Standard Library**, you can find `importlib`, which is a module that provides `import_module()`.

With `import_module()`, you can emulate an import operation and, therefore, execute any module or script. Take a look at this example:

```python
>>> import importlib
>>> importlib.import_module('hello')
Hello World!
<module 'hello' from '/home/username/hello.py'>
```

Once you’ve imported a module for the first time, you won’t be able to continue using import to run it. In this case, you can use importlib.reload(), which will force the interpreter to re-import the module again, just like in the following code:

```python
>>> import hello  # First import
Hello World!
>>> import hello  # Second import, which does nothing
>>> import importlib
>>> importlib.reload(hello)
Hello World!
<module 'hello' from '/home/username/hello.py'>
```

An important point to note here is that the argument of `reload()` has to be the name of a module object, not a string:

```python
>>> importlib.reload('hello')
Traceback (most recent call last):
    ...
TypeError: reload() argument must be a module
```

`importlib.reload()` comes in handy when you are modifying a module and want to test if your changes work, without leaving the current interactive session.

### Using runpy.run_module() and runpy.run_path()
The Standard Library includes a module called `runpy`. In this module, you can find `run_module()`, which is a function that allows you to run modules without importing them first. This function returns the globals dictionary of the executed module.

Here’s an example of how you can use it:

```python
>>> runpy.run_module(mod_name='hello')
Hello World!
{'__name__': 'hello',
    ...
'_': None}}
```

The module is located using a standard **import** mechanism and then executed on a fresh module namespace.

The first argument of `run_module()` must be a string with the absolute name of the module (without the **.py** extension).

On the other hand, runpy also provides `run_path()`, which will allow you to run a module by providing its location in the filesystem:

```python
>>> import runpy
>>> runpy.run_path(file_path='hello.py')
Hello World!
{'__name__': '<run_path>',
    ...
'_': None}}
```

Like `run_module()`, `run_path()` returns the **globals** dictionary of the executed module.

The `file_path` parameter must be a string and can refer to the following:

- The location of a Python source file
- The location of a compiled bytecode file
- The value of a valid entry in the `sys.path`, containing a `__main__` module (`__main__.py` file)

### Hacking exec()
So far, you’ve seen the most commonly used ways to run Python scripts. In this section, you’ll see how to do that by using `exec()`, which is a built-in function that supports the dynamic execution of Python code.

`exec()` provides an alternative way for running your scripts:

```python
>>> exec(open('hello.py').read())
'Hello World!'
```

This statement opens **hello.py**, reads its content, and sends it to `exec()`, which finally runs the code.

The above example is a little bit out there. It’s just a “hack” that shows you how versatile and flexible Python can be.

For more information on running your scripts from an IDE, Text Editor or File Manager, take a look at: https://realpython.com/run-python-scripts/
