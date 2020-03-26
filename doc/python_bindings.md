# Calling a C library
Python bindings allow you to call functions and pass data from Python to C, letting you take advantage of the strengths of both languages.

# Why call C from Python
There are several situations where creating Python bindings to call a C library is a great idea:
- You already have a large, tested, stable library written in C
- You want to speed up a particular section of your Python code
- You want to use Python test tools

# Exploring the possible tools/methods
## ctypes
There are times when all you need to do is invoke some system calls or a few C library functions and you want to avoid the overhead of writing two different languages. In this case you can use a Python library like `ctypes`.

## CFFI


## Cython


## C extension module
