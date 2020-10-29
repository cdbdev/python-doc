# Introduction

Composite Data Types are composed of two or more other data types.

# List

Lists are just like dynamic sized arrays, declared in other languages (vector in C++ and ArrayList in Java). Lists need not be homogeneous always which makes it the most powerful tool in Python. The main characteristics of lists are:

- defined under square brackets[ ]
- mutable
- ordered (they retain items in the order you insert them)
- can store any type of element

Example:
# Creating a List of numbers 
list = [10, 20, 14] 
print("\nList of numbers: ") 
print(list) 


# Tuple

Tuple is a collection of Python objects much like a list. The sequence of values stored in a tuple can be of any type, and they are indexed by integers. Values of a tuple are syntactically separated by ‘commas’. Although it is not necessary, it is more common to define a tuple by closing the sequence of values in parentheses. The main characteristics of tuples are: 

- immutable
- ordered (they retain items in the order you insert them)
- defined under parenthesis()
- can store any type of element
- operations have smaller size than that of list, which makes it a bit faster but not that much to mention about until you have a huge number of elements

Example:
# Creating a Tuple of numbers 
tuple = (10, 20, 14)
print("\nTuple of numbers: ") 
print(List) 


# Set

Set is an unordered collection of data type that is iterable, mutable, and has no duplicate elements. The major advantage of using a set, as opposed to a list, is that it has a highly optimized method for checking whether a specific element is contained in the set. The main characteristics of set are:

- unordered (the order in which the elements are added into the set is not fixed, it can change frequently)
- defined under curly braces{ }
- mutable, however, only immutable objects can be stored in it (Numbers, strings, and tuples are immutable)

Example:
# set of mixed datatypes
my_set = {1.0, "Hello", (1, 2, 3)}
print(my_set)


# Different Use Cases

At first sight, it might seem that lists can always replace tuples. But tuples are extremely useful data structures

1. Using a tuple instead of a list can give the programmer and the interpreter a hint that the data should not be changed.

2. Tuple can also be used as key in dictionary due to their hashable and immutable nature whereas Lists are not used as key in a dictionary because list can’t handle __hash__() and have mutable nature.

For Example:
key_val= {('alpha','bravo'):123} #Valid
key_val = {['alpha','bravo']:123} #Invalid

3. Tuples are commonly used as the equivalent of a dictionary without keys to store data. For Example:

[('Swordfish', 'Dominic Sena', 2001), ('Snowden', ' Oliver Stone', 2016), ('Taxi Driver', 'Martin Scorsese', 1976)]

Above example contains tuples inside list which has a list of movies.
