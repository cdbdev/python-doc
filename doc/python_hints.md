# Introduction
Type hinting is a formal solution to statically indicate the type of a value within your Python code. It was specified in `PEP 484` and introduced in `Python 3.5`.

The Python runtime does not enforce function and variable type annotations. They can be used by third party tools such as type checkers, 
IDEs, linters, etc.

The function below takes and returns a string and is annotated as follows:
```python
def greeting(name: str) -> str:
    return 'Hello ' + name
```
The name: `str` syntax indicates the name argument should be of type str. The `->` syntax indicates the `greet()` function will return a string.

...TODO


# mypy
With the development of PEP 484 is that it was developed in conjunction with `mypy`, a project out of DropBox, which checks the types as you run the program. Remember that types are not checked at run-time. You’ll only get an issue if you try to run a method on a type that’s incompatible.


# When to use
As Guido and the mypy docs say:  
> The aim of mypy is not to convince everybody to write statically typed Python – static typing is entirely optional, now and in the future. The goal is to give more options for Python programmers, to make Python a more competitive alternative to other statically typed languages in **large projects**, to improve programmer productivity, and to improve software quality.

Because of the overhead of setting up mypy and thinking through the types that you need, type hints don’t make sense for smaller codebases. What’s a small codebase? Probably anything under 1k LOC, conservatively speaking. 

For larger codebases, places where you’re working with others, collaborating, and packages, places where you have version control and continuous integration system, it makes sense and could save a lot of time.

# References
http://veekaybee.github.io/2019/07/08/python-type-hints/
