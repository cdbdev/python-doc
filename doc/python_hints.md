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

...
