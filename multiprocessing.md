# Using Multiprocessing to Make Python Code Faster
The "multi" in multiprocessing refers to the multiple cores in a computer’s central processing unit (CPU). Computers originally had only one CPU core or processor, which is the unit that makes all our mathematical calculations possible. Today, computers typically have anywhere from 2 to 128 cores, meaning that taking advantage of more than one can dramatically improve processing time.

In Python, single-CPU use is caused by the **global interpreter lock (GIL)**, which allows only one thread to carry the Python interpreter at any given time. The GIL was implemented to handle a memory management issue, but as a result, Python is limited to using a single processor.

![](python_GIL.jpeg)

# Multiprocessing can dramatically improve processing speed
Bypassing the GIL when executing Python code allows the code to run faster because we can now take advantage of multiprocessing. Python’s built-in multiprocessing module allows us to designate certain sections of code to bypass the GIL and send the code to multiple processors for simultaneous execution.

![](python_GIL_bypass.jpeg)

In this simplified example, assuming all three threads had identical runtimes, the multiprocessing solution would cut total execution time by a third. But this reduction isn’t exactly proportionate to the number of processors available because of the overhead involved in creating multiprocessing processes, but the gains represent a significant improvement over single-core operations.

# Three requirements for multiprocessing
Before you can begin multiprocessing, you need to pick which sections of code to multiprocess. These sections of code must meet the following criteria:

1. Must not be reliant on previous outcomes
2. Does not need to be executed in a particular order
3. Does not return anything that would need to be accessed later in the code

_For reference see: https://medium.com/@urban_institute/using-multiprocessing-to-make-python-code-faster-23ea5ef996ba_
