# Python distribute standalone apps

Are you jealous of **Go** developers building an executable and easily shipping it to users? Wouldn’t it be great if your users could 
**run your application without installing anything**? That is the dream, and PyInstaller is one way to get there in the Python ecosystem.

There are countless tutorials on how to set up **virtual environments**, **manage dependencies**, and **publish to PyPI**, which is 
useful when you’re creating Python libraries. There is much less information for developers building Python applications. This article 
is for developers who want to distribute applications to users who may or may not be Python developers.

**PyInstaller** gives you the ability to create a folder or executable that users can immediately run without any extra installation. To 
fully appreciate PyInstaller’s power, it’s useful to revisit some of the distribution problems PyInstaller helps you avoid.

## Distribution Problems

Setting up a Python project can be frustrating, especially for non-developers. Often, the setup starts with opening a Terminal, which 
is a non-starter for a huge group of potential users. This roadblock stops users even before the installation guide delves into the 
complicated details of virtual environments, Python versions, and the myriad of potential dependencies.

Think about what you typically go through when setting up a new machine for Python development. It probably goes something like this:

- Download and install a specific version of Python
- Set up pip
- Set up a virtual environment
- Get a copy of your code
- Install dependencies

Stop for a moment and consider if any of the above steps make any sense if you’re not a developer, let alone a Python developer. 
Probably not.

These problems explode if your user is lucky enough to get to the dependencies portion of the installation. This has gotten much better 
in the last few years with the prevalence of wheels, but some dependencies still require C/C++ or even FORTRAN compilers!

This barrier to entry is way too high if your goal is to make an application available to as many users as possible. 
This is where PyInstaller comes in to play.

For more information, take a look at: https://realpython.com/pyinstaller-python/

## Standalone app issues

Packaging and application distribution is a hard problem on multiple dimensions. For Python, large aspects of this problem space are 
more or less solved if you are distributing open source Python libraries and your target audience are developers (use pip and PyPI). 
But if you are distributing Python applications - standalone executables that use Python - your world can be much more complicated.

One of the primary reasons why distributing Python applications is difficult is because of the complex and often sensitive relationship 
between a Python application and the environment it runs in.

Possible scenario's:
- If your application doesn't distribute the Python interpreter, you are at the whims of the Python interpreter provided by the host machine
- even the presence of a supposedly compatible version of the Python interpreter isn't a guarantee for success! For example, the Python interpreter could have a built-in extension that links against an old version of a library ( ex. SQlite)
- the interpreter could have modifications or extra installed packages interfering with the operation of your application
- even if the Python interpreter on the target machine is fully compatible, getting your code to run on that interpreter could be difficult! Several Python applications leverage compiled extensions linking against Python's C API. Distributing the precompiled form of the extension can be challenging, especially when your code needs to link against 3rd party libraries, which may conflict with something on the target system. And, the precompiled extensions need to be built in a very delicate manner to ensure they can run on as many target machines as possible

Knowing these scenario's will help you build future standalone apps so you won't run into unexpected problems.
