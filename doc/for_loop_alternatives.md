# 1. You (Probably) Don't need For-Loops
Looping over Python large datasets like arrays, lists, or dictionaries, can be slow. Fortunately, there are already great tools that are built into Python to speed up your code! 

## 1.2 So what makes a loop in Python any slower than it would be in native machine code?
It’s often necessary to work with very large datasets. Those large datasets get read directly into memory, and are stored and processed as Python arrays, lists, or dictionaries.

Working with such huge arrays can be time consuming; really that’s just the nature of the problem. You have thousands, millions, or even billions of data points. Every microsecond added to the processing of a single one of those points can drastically slow you down as a result of the large scale of the data you’re working with.



MAPS:
You can think of map as a for moved into C code. The only restriction is that the "loop body" of map must be a function call. 
