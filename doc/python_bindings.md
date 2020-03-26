# Calling a C library
Python bindings allow you to call functions and pass data from Python to C, letting you take advantage of the strengths of both languages.

# Why call C from Python
There are several situations where creating Python bindings to call a C library is a great idea:
- You already have a large, tested, stable library written in C
- You want to speed up a particular section of your Python code
- You want to use Python test tools

# Exploring the possible tools/methods
## ctypes
This is a tool in the standard library for creating Python bindings. It provides a low-level toolset for loading shared libraries and marshalling data between Python and C.

There are times when all you need to do is invoke some system calls or a few C library functions and you want to avoid the overhead of writing two different languages. In this case you can use a Python library like `ctypes`.

The biggest advantage ctypes has over the other tools you’ll examine here is that it’s built into the standard library. It also requires no extra steps, as all of the work is done as part of your Python program. However, more complex tasks grow cumbersome with the lack of automation. In the next section, you’ll see a tool that adds some automation to the process.

## CFFI
CFFI is the **C Foreign Function Interface** for Python. It takes a more automated approach to generate Python bindings. CFFI has multiple ways in which you can build and use your Python bindings. There are two different options to select from, which gives you four possible modes:
- ABI vs API: API mode uses a C compiler to generate a full Python module, whereas ABI mode loads the shared library and interacts with it directly. Without running the compiler, getting the structures and parameters correct is error-prone. The documentation heavily recommends using the API mode
- in-line vs out-of-line: The difference between these two modes is a trade-off between speed and convenience:
  - tse

## Cython


## C extension module
