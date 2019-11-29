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
