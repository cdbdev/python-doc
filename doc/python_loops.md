# 1. Introduction
Python provides a few different ways to create **lists**. The most common way to do this is through the **for loop**. 

Example:
```python
squares = []
for i in range(10):
    squares.append(i*i)
```

These loops have their use, but they also have some disadvantages:

- multiple levels of indentation (especially with nested loops)
- code readability suffers in large blocks
- can be slow

In case you encounter one or more of the above issues, there are several alternatives.

# 2. List Comprehension
List comprehension provides an alternative approach of making lists. They are easier to read and understand.
You can rewrite the loop from the first example in just a single line of code:
```python
squares = [i*i for i in range(10)]
```
Rather than creating an empty list and adding each element to the end, you simply define the list and its content at the same time.

List Comprehension is also often described as being more _Pythonic_ than loops because you can use them in many different situations. List Comprehension can, in addition to standard list creation, also be used for mapping and filtering. You don't have to use a different approach for each scenario. 

> Note: List Comprehension is just a _syntactic sugar_ around a regular loop and you use it to avoid extra function calls like appending to lists, etc.

# 3. Map
`map()` provides a third way of making lists. It's based on **functional progamming**. You pass in a **function** and an **interable**, and `map()` will create an object.

```python
def square(item):
    return item*item
square_iterator = map(square,range(10))
```
It is important to note that the `square_iterator` object is not a list, but returns a map object(which is an iterator) of the results. In order to get a list, we have to convert it:
```python
list(square_iterator)
```
Another important remark with map is that **builtin** functions run faster than **hand-built** equivalents. For example:
```python
import operator
map(operator.add, range(10), range(10))
```
is faster than:
```python
map(lambda x, y: x+y, range(10), range(10))
```

# 4. So which one to choose
Whenever you have to choose a list creation method, try multiple implementations and consider what's easiest to read and understand in your specific scenario. 

If performance is important, then you can use profiling tools to give you actionable data instead of relying on hunches of guesses about what works best.


## Appendix A: So what makes a for-loop in Python any slower than it would be in native machine code?
A python for-loop is actually a for-each loop and for one cannot be compared with the **Three-Expression Loop** of C. Python only implements the **collection-based iteration**.

It’s often necessary to work with **very large datasets**. Those large datasets get read directly into memory, and are stored and processed as Python arrays, lists, or dictionaries. Working with such huge arrays can be time consuming; really that’s just the nature of the problem. You have thousands, millions, or even billions of data points. Every microsecond added to the processing of a single one of those points can drastically slow you down as a result of the large scale of the data you’re working with.
