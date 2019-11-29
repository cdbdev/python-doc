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

## Run Python Code Interactively

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

## How Does the Interpreter Run Python Scripts?
When you try to run Python scripts, a multi-step process begins. In this process the interpreter will:
1. **Process the statements of your script in a sequential fashion**
2. **Compile the source code to an intermediate format known as bytecode**  This bytecode is a translation of the code into a lower-level language that’s platform-independent. Its purpose is to optimize code execution. So, the next time the interpreter runs your code, it’ll bypass this compilation step. Strictly speaking, this code optimization is only for modules (imported files), not for executable scripts.  

3. **Ship off the code for execution**  At this point, something known as a Python Virtual Machine (PVM) comes into action. The PVM is the runtime engine of Python. It is a cycle that iterates over the instructions of your bytecode to run them one by one.

The PVM is not an isolated component of Python. It’s just part of the Python system you’ve installed on your machine. Technically, the PVM is the last step of what is called the Python interpreter.

The whole process to run Python scripts is known as the **Python Execution Model**.

> **Note**: This description of the Python Execution Model corresponds to the core implementation of the language, that is, CPython. As this is not a language requirement, it may be subject to future changes.

## Run Python Scripts Using the Command-Line
A Python interactive session will allow you to write a lot of lines of code, but once you close the session, you lose everything you’ve written. That’s why the usual way of writing Python programs is by using plain text files. By convention, those files will use the .py extension. (On Windows systems the extension can also be .pyw.)

### Using the python Command
To run Python scripts with the python command, you need to open a command-line and type in the word python, or python3 if you have both versions, followed by the path to your script, just like this:

```bash
$ python3 hello.py
Hello World!
```

This is the most basic and practical way to run Python scripts.

### Running Modules With the -m Option
Python offers a series of command-line options that you can use according to your needs. For example, if you want to run a Python module, you can use the command `python -m <module-name>`.

The `-m` option searches `sys.path` for the module name and runs its content as `__main__`:

```bash
$ python3 -m hello
Hello World!
```

### Using the Script Filename
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

### Run Python Scripts Interactively
It is also possible to run Python scripts and modules from an interactive session. This option offers you a variety of possibilities.

**Taking Advantage of import**
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

**Using importlib and imp**
