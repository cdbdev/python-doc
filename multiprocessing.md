# Using Multiprocessing to Make Python Code Faster
The "multi" in multiprocessing refers to the multiple cores in a computer’s central processing unit (CPU). Computers originally had only one CPU core or processor, which is the unit that makes all our mathematical calculations possible. Today, computers typically have anywhere from 2 to 128 cores, meaning that taking advantage of more than one can dramatically improve processing time.

In Python, single-CPU use is caused by the **global interpreter lock (GIL)**, which allows only one thread to carry the Python interpreter at any given time. The GIL was implemented to handle a memory management issue, but as a result, Python is limited to using a single processor.

![](python_GIL.jpeg)

_For reference see: https://medium.com/@urban_institute/using-multiprocessing-to-make-python-code-faster-23ea5ef996ba_
