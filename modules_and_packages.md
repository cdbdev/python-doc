# 1. Naming conventions
Python style is governed largely by a set of documents called **Python Enhancement Proposals**, abbreviated **PEP**.
Not all PEPs are actually adopted, of course - that's why they're called "Proposals" - but some are. you can browse
the master PEP index on the official Python website. This index is formally referred to as `PEP 0`.

Right now we're mainly concerned with **PEP 8**, first authored by the Python language creator Guido van Rossum back 
in 2001. It is the document which officially outlines the coding style all Python developers should generally follow. 
Keep it under your pillow! Learn it, follow it, encourage others to do the same.

(Side Note: PEP 8 makes the point that there are always exceptions to style rules. It's a guide, not a mandate.)

Right now, we're chiefly concerned with the section entitled `Package and Module Names`:
> Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability. 
Python packages should also have short, all-lowercase names, although the use of underscores is discouraged.

We'll get to what exactly modules and packages are in a moment, but for now, understand that **modules are named by 
filenames**, and **packages are named by their directory name**.

In other words, **filenames should be all lowercase, with underscores if that improves readability**. Similarly, **directory 
names should be all lowercase, without underscores if at all avoidable**. To put that another way...

- Do This: `awesomething/data/load_settings.py`
- NOT This: `awesomething/Data/LoadSettings.py`

# 2. Modules and Packages
`Any Python (.py) file is a module, and a bunch of modules in a directory is a package.`

Besides collecting modules into a directory, there is another requirement to define a directory as a package.
The key to designing a package is by adding an `__init__.py` file into that directory. This will define what gets 
brought into the namespace with the import statement. You actually don't have to put anything into that file. It just 
has to be there. It can also execute initialization code for the package or set the `__all__` variable, but more on 
that later.

If you do forget `__init__.py` in your package, it will be defined as an implicit namespace package. 

> _Remark:_ As of **PEP 420 -- Implicit Namespace Packages**, the `__init__.py` file is not always necessary. 
See: [Python package init file](python_package_init_file.md).

Let's take a look at a recommended structure:

```
examplepackage-git/
├── examplepackage
│   ├── __init__.py
│   ├── app.py
│   ├── common
│   │   ├── __init__.py
│   │   ├── enums.py
│   ├── model
│   │   ├── __init__.py
│   │   ├── nice_model.py
│   ├── tests
│   │   ├── __init__.py
│   │   ├── test_project.py
│   ├── resources
├── LICENSE.md
├── MANIFEST.in
├── README.md
├── setup.py
└── venv
    ├── bin
    ├── include
    ├── lib
    ├── lib64 -> lib
    ├── pyvenv.cfg
    └── share
```

You'll see that there is one top-level package called `examplepackage`, with three sub-packages: 
- common
- model
- tests

We also have the directory `resources`, but that only contains game audio, images, etc. It is NOT a package, as it 
doesn't contain an `__init__.py`.

go to: https://dev.to/codemouse92/dead-simple-python-project-structure-and-imports-38c6
