# 1. You (Probably) Don't need For-Loops
The for statement is most commonly used. It loops over the elements of a sequence, assigning each to the loop variable.  
Looping over Python large datasets like arrays, lists, or dictionaries, can be slow. Fortunately, there are already great tools that are built into Python to speed up your code! 

## 1.2 So what makes a loop in Python any slower than it would be in native machine code?
Python is a very high level and dynamic language. It’s variables are references to objects and any reference might be repeatedly changed to objects of entirely different types (classes) throughout the execution of any loop. These changes might occur as side effects of any function or method called within any iteration of any loop … including indirectly… through arbitrary levels of nesting.

Handling this comes at a cost, That cost is minimal for most code over most common datasets. But it’s the main thing that makes Python “slow” compared to what’s possible with compiled native code,



MAPS:
You can think of map as a for moved into C code. The only restriction is that the "loop body" of map must be a function call. 
