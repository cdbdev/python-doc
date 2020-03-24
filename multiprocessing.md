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

# Example
```python
import time

def basic_func(x):
    if x == 0:
        return 'zero'
    elif x%2 == 0:
        return 'even'
    else:
        return 'odd'
    
starttime = time.time()
for i in range(0,10):
    y = i*i
    time.sleep(2)
    print('{} squared results in a/an {} number'.format(i, basic_func(y)))
    
print('That took {} seconds'.format(time.time() - starttime))
```
This example took about **20.019609451293945** seconds.

The traditional for-loop iteration goes through the list one by one and performs the functions on each item individually. In this model, the loops use only 30 to 40 percent of the available CPU power. If we were to use multiprocessing, the function would be executed on multiple list items at once and up to 100 percent of the CPU could be used on a multicore machine. Best of all, we would see a dramatic reduction in execution time.

## How to use multiprocessing: The Process class and the Pool class
The multiprocessing Python module contains _two classes_ capable of handling tasks. The Process class sends each task to a different processor, and the Pool class sends sets of tasks to different processors. We will show how to multiprocess the example code using both classes. Although both classes provide a similar speed increase, the Process class is more efficient in this case because there are not many processes to execute.

**Pool is most useful for large amounts of processes where each process can execute quickly, while Process is most useful for a small number of processes where each process would take a longer time to execute.**

To use the **Process class**, place the functions and calculations that are done on each list item in its own function that will take a list item as one of its arguments. Next, import the multiprocessing module, create a new process for each list item, and trigger each process in one call. We keep track of these processes by making a list and adding each process to it. After creating all the processes, take the separate output of each CPU and join them into a single list.

```python
import time
import multiprocessing 

def basic_func(x):
    if x == 0:
        return 'zero'
    elif x%2 == 0:
        return 'even'
    else:
        return 'odd'

def multiprocessing_func(x):
    y = x*x
    time.sleep(2)
    print('{} squared results in a/an {} number'.format(x, basic_func(y)))
    
if __name__ == '__main__':
    starttime = time.time()
    processes = []
    for i in range(0,10):
        p = multiprocessing.Process(target=multiprocessing_func, args=(i,))
        processes.append(p)
        p.start()
        
    for process in processes:
        process.join()
        
    print('That took {} seconds'.format(time.time() - starttime))
```
This example took about **2.226956844329834** seconds.

To use the **Pool class**, we also have to create a separate function that takes a list item as an argument like we did when using Process. Then, using the multiprocessing module, create a Pool object called pool. This object has a function called map, which takes the function we want to multiprocess and the list as arguments and then iterates through the list for that function. After calling the function map, close the object to allow for a clean shutdown.

```python
import time
import multiprocessing 

def basic_func(x):
    if x == 0:
        return 'zero'
    elif x%2 == 0:
        return 'even'
    else:
        return 'odd'

def multiprocessing_func(x):
    y = x*x
    time.sleep(2)
    print('{} squared results in a/an {} number'.format(x, basic_func(y)))
    
if __name__ == '__main__':
    
    starttime = time.time()
    pool = multiprocessing.Pool()
    pool.map(multiprocessing_func, range(0,10))
    pool.close()
    print('That took {} seconds'.format(time.time() - starttime))
```
This example took about **4.165083408355713** seconds.

## The verdict
Unless you are running a machine with more than 10 processors, the Process code should run faster than the Pool code. Process sends code to a processor as soon as the process is started. Pool sends a code to each available processor and doesn’t send any more until a processor has finished computing the first section of code. Pool does this so that processes don’t have to compete for computing resources, but this makes it slower than Process in cases where each process is lengthy. For an example where Pool is faster than Process, remove the line `time.sleep(2)` in `multiprocessing_func` in both the Process and Pool codes.

The multiprocessed code doesn’t execute in the same order as serial execution. There’s no guarantee that the first process to be created will be the first to start or complete. As a result, multiprocessed code usually executes in a different order each time it is run, even if each result is always the same.

With multiprocessing, using higher-level programming languages doesn’t necessarily mean sacrificing speed. Certain aspects of the code can be run in parallel, which allows us to do our work faster and more efficiently. Thanks to multiprocessing, we cut down runtime of cloud-computing system code from more than 40 hours to as little as 6 hours. In another project, we cut down runtime from 500 hours to just 4 on a 128-core machine.

# What about 'threads'
A thread (threading module) is a separate flow of execution. This means that your program will have two things happening at once. But for most Python 3 implementations the different threads do not actually execute at the same time: they merely appear to.

It’s tempting to think of threading as having two (or more) different processors running on your program, each one doing an independent task at the same time. That’s almost right. The threads may be running on different processors, but they will only be running one at a time.

Getting multiple tasks running simultaneously requires a non-standard implementation of Python, writing some of your code in a different language, or using multiprocessing which comes with some extra overhead.

Because of the way CPython implementation of Python works, threading may not speed up all tasks. This is due to interactions with the GIL that essentially limit one Python thread to run at a time.

## Example
```python
import threading
import time

exitFlag = 0

class myThread (threading.Thread):
   def __init__(self, threadID, name, counter):
      threading.Thread.__init__(self)
      self.threadID = threadID
      self.name = name
      self.counter = counter
   def run(self):
      print "Starting " + self.name
      print_time(self.name, 5, self.counter)
      print "Exiting " + self.name

def print_time(threadName, counter, delay):
   while counter:
      if exitFlag:
         threadName.exit()
      time.sleep(delay)
      print "%s: %s" % (threadName, time.ctime(time.time()))
      counter -= 1

# Create new threads
thread1 = myThread(1, "Thread-1", 1)
thread2 = myThread(2, "Thread-2", 2)

# Start new Threads
thread1.start()
thread2.start()

print "Exiting Main Thread"
```

Output:
```
Starting Thread-1
Starting Thread-2
Exiting Main Thread
Thread-1: Thu Mar 21 09:10:03 2013
Thread-1: Thu Mar 21 09:10:04 2013
Thread-2: Thu Mar 21 09:10:04 2013
Thread-1: Thu Mar 21 09:10:05 2013
Thread-1: Thu Mar 21 09:10:06 2013
Thread-2: Thu Mar 21 09:10:06 2013
Thread-1: Thu Mar 21 09:10:07 2013
Exiting Thread-1
Thread-2: Thu Mar 21 09:10:08 2013
Thread-2: Thu Mar 21 09:10:10 2013
Thread-2: Thu Mar 21 09:10:12 2013
Exiting Thread-2
```

## What Should You Use?
If your code has a lot of I/O or Network usage:
- Multithreading is your best bet because of its low overhead

If you have a GUI:
- Multithreading so your UI thread doesn't get locked up

If your code is CPU bound:
- You should use multiprocessing (if your machine has multiple cores)

_For reference see: https://medium.com/@urban_institute/using-multiprocessing-to-make-python-code-faster-23ea5ef996ba_
