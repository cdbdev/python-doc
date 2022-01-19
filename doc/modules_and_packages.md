# Metadata


How modules and packages are managed by Python.  
Working with imports

---


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
│   │   ├── utils.py
│   ├── model
│   │   ├── __init__.py
│   │   ├── nice_model.py
│   │   ├── sub_model.py
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

The 'tests' directory here is part of the package. You can also put this directory outside the application code.  
For more info see: [Python testing](python_testing.md).

We also have the directory `resources`, but that only contains images, etc. It is NOT a package, as it doesn't contain 
an `__init__.py`.

# 3. How import Works
## 3.1 sys.path
Python `imports` work by searching the directories listed in `sys.path` which is populated using the **current working 
directory**, followed by directories listed in your `PYTHONPATH` environment variable, followed by installation-dependent 
default paths, which are controlled by the `site module`.

You are allowed to modify `sys.path` during run-time. Just be sure to modify it _before_ you call `import`. It will 
search the directories in order, stopping at the first place it finds the specified modules.

**PYTHONPATH**  
`PYTHONPATH` is related to `sys.path` very closely. `PYTHONPATH` is an environment variable that you set before running 
the Python interpreter. `PYTHONPATH`, if it exists, should contain directories that should be searched for modules when 
using `import`. If `PYTHONPATH` is set, Python will include the directories in `sys.path` for searching. Use a semicolon to 
separate multiple directories.

So, in order to import modules or packages, they need to reside in one of the paths listed in `sys.path`. You can modify 
the `sys.path` list manually if needed from within Python. It is just a regular `list` so it can be modified in all the 
normal ways. For example, you can append to the end of the list using `sys.path.append()` or to insert in an arbitrary 
position using `sys.path.insert()`.

**The site module**  
You can also use the site module to modify sys.path:
```python
import site
import sys

site.addsitedir('/the/path')  # Always appends to end
print(sys.path)
```

**PYTHONHOME**  
The `PYTHONHOME` environment variable is similar to `PYTHONPATH` except it should define where the standard libraries 
are. If `PYTHONHOME` is set, it will assume some default paths relative to the home, which can be supplemented with 
`PYTHONPATH`.

## 3.2 Importing a module equals running the module
Example of `import` statement with the 'Regular expression operations' module

```python
import re
```

It is helpful to know that, when we import a module, we are actually running it. This means that any import statements 
in the module are also being run.

For example, `re.py` has several import statements of its own, which are executed when we say `import re`. That doesn't 
mean they're available to the file we imported `re` from, but it does mean those files have to exist. If (for some 
_unlikely_ reason) `enum.py` got deleted on your environment, and you ran `import re`, it would fail with an error...

```bash
Traceback (most recent call last):
File "example.py", line 1, in
import re
File "re.py", line 122, in
import enum
ModuleNotFoundError: No module named 'enum'
```

## 3.3 The module __file__ attribute
When you import a module, you usually can check the __file__ attribute of the module to see where the module is in your 
filesystem:

```bash
> import numpy
> numpy.__file__
'/usr/local/lib/python2.7/dist-packages/numpy/__init__.pyc'
```

However, the Python docs state that:  
> The file attribute is not present for C modules that are statically linked into the interpreter; for extension 
modules loaded dynamically from a shared library, it is the pathname of the shared library file.

So, for example this doesn't work:

```bash
> import sys
> sys.__file__
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'module' object has no attribute '__file__'
```

It makes sense that the `sys` module is statically linked to the interpreter - it is essentially part of the interpreter!

# 4. Import Dos and Don'ts
There are actually a number of ways of importing, but most of them should rarely, if ever be used.

For all of the examples below, we'll imagine that we have a file called `smart_door.py`:

```python
# smart_door.py
def close():
    print("Ahhhhhhhhhhhh.")

def open():
    print("Thank you for making a simple door very happy.")
```

Just for example, we will run the rest of the code in this section in the Python interactive shell, from the same 
directory as `smart_door.py`.

If we want to run the function `open()`, we have to first import the module `smart_door`. The easiest way to do this is...

```python
import smart_door
smart_door.open()
smart_door.close()
```

We would actually say that `smart_door` is the namespace of `open()` and `close()`. Python developers really like 
namespaces, because they make it obvious where functions and whatnot are coming from.

(By the way, don't confuse _namespace_ with _implicit namespace package_. They're two different things.)

At a certain point, however, namespaces can become a pain, especially with nested packages. 
`foo.bar.baz.whatever.doThing()` is just ugly. Thankfully, we do have a way around having to use the namespace 
_every time_ we call the function.

If we want to be able to use the `open()` function without constantly having to precede it with its module name, we can 
do this instead...

```python
from smart_door import open
open()
```

Note, however, that neither `close()` nor `smart_door.close()` will work in that last scenario, because we didn't 
import the function outright. To use it, we'd have to change the code to this...

```python
from smart_door import open, close
open()
close()
```

In that terrible nested-package nightmare earlier, we can now say `from foo.bar.baz.whatever import doThing`, and then 
just use `doThing()` directly. Alternatively, if we want a LITTLE bit of namespace, we can say `from foo.bar.baz import 
whatever`, and say `whatever.doThing()`.

The import system is _deliciously_ flexible like that.

Before long, though, you'll probably find yourself saying "But I have hundreds of functions in my module, and I want to 
use them all!" This is the point at which many developers go off the rails, by doing this...

```python
from smart_door import *
```

**This is very, very bad!** Simply put, it imports everything in the module directly, and that's a problem. Imagine the 
following code...

```python
from smart_door import *
from gzip import *
open()
```

What do you suppose will happen? The answer is, `gzip.open()` will be the function that gets called, since that's the 
last version of `open()` that was imported, and thus defined, in our code. `smart_door.open()` has been **shadowed** 
- we can't call it as `open()`, which means we effectively can't call it at all.

Of course, since we usually don't know, or at least don't remember, _every single function_, class, and variable in every 
module that gets imported, we can easily wind up with a whole lot of messes.

The Zen of Python addresses this scenario as well...
> Explicit is better than implicit.

You should never have to _guess_ where a function or variable is coming from. Somewhere in the file should be code that 
_explicitly_ tells us where it comes from. The first two scenarios demonstrate that.

I should also mention that the earlier `foo.bar.baz.whatever.doThing()` scenario is something Python developers do NOT 
like to see. 

Also from the _Zen of Python..._
> Flat is better than nested.

Some nesting of packages is okay, but when your project starts looking like an elaborate set of Matryoshka dolls, 
you've done something wrong. Organize your modules into packages, but keep it reasonably simple.

# 5. Importing Within Your Project
Below an example project file structure.

```
examplepackage-git/
├── examplepackage
│   ├── __init__.py
│   ├── app.py
│   ├── __main__.py
│   ├── common
│   │   ├── __init__.py
│   │   ├── utils.py
│   ├── model
│   │   ├── __init__.py
│   │   ├── nice_model.py
│   │   ├── sub_model.py
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

In my `nice_model` module, defined by `examplepackage/model/nice_model.py`, I want to use a `MyGenerator` class. 
That class is defined in `examplepackage/common/utils.py`. How do I get to it?	

Because I defined `examplepackage` as a package, and organized my modules into subpackages, it's actually pretty easy. 
In `nice_model.py`, I say...

```python
from examplepackage.common.utils import MyGenerator
```

This is called an **absolute import**. It starts at the top-level package, examplepackage, and walks down into the 
`common` package, where it looks for `utils.py`. 

So if your top-level package is located at:

```bash
/home/chris/dev/python/examplepackage
```

You would have to navigate to this location and run the modules from there, even if you want to run a module that 
resides in a subdirectory. 

To execute a module individually, you can use the `python -m` command. Just make sure you use a `.` to navigate through
subfolders instead of `/`. The reason for this is because Python threats folders that contain an `__init__.py` file as 
packages instead of directories. Also, since you run it as a module, you do not have to specify the `.py` extension.

Example: 

```bash
cd /home/chris/dev/python/examplepackage
python -m model.nice_model
```

Some developers come to me with import statements more like `from common.utils import MyGenerator`, and wonder why it 
doesn't work. Simply put, the `model` package (where `nice_model.py` lives) has no knowledge of its sibling 
packages. It does, however, know about its parents. Because of this, Python has something called **relative imports** 
that lets us do the same thing like this instead...

```python
from ..common.utils import MyGenerator
```

The `..` means "this package's direct parent package", which in this case, is `examplepackage`. So, the import steps 
back one level, walks down into `common`, and finds `utils.py`.

There's a lot of debate about whether to use absolute or relative imports. Personally, I prefer to use absolute imports 
whenever possible, because it makes the code a lot more readable. You can make up your own mind, however. The only 
important part is that the result is _obvious_ - there should be no mystery where anything comes from.

There is one other lurking gotcha here! In `examplepackage/model/nice_model.py`, I have this line:

```python
from examplepackage.model.sub_model import MySubModel
```

Surely, since both these modules are in the same package, we should be able to just say `from sub_model import 
MySubModel`, right?

_Wrong!_ It will actually fail to locate `sub_model.py`. This is because we are running the top-level package 
`examplepackage`, which means the **search path** (where Python looks for modules, and in what order) works differently.

However, we can use a relative import instead:

```python
from .sub_model import MySubModel
```

In that case, the single `.` means "this package".

If you're familiar with the typical UNIX file system, this should start to make sense. `..` means "back one level", and 
`.` means "the current location". Of course, Python takes it one step further: `...` means "back two levels", `....` is 
"back three levels", and so forth.

However, keep in mind that those "levels" aren't just plain directories, here. They're packages. If you have two 
distinct packages in a plain directory that is NOT a package, you can't use relative imports to jump from one to 
another. You'll have to work with the Python search path for that. 

# 6. `__main__.py`
This is a special file in our top-level package that is executed when we run the package directly with Python. 
The `examplepackage` package can be run from the root of the repository with 
`python -m examplepackage`.

Here's the content of that file:

```python
from examplepackage import app

if __name__ == '__main__':
    app.run()
```

Yep, that's actually it! I'm importing my module `app` from the top-level package `examplepackage`.

I could also have said `from . import app` instead. Alternatively, if I wanted to just say `run()` instead of 
`app.run()`, I could have done `from examplepackage.app import run` or `from .app import run`. In the end, it doesn't 
make much technical difference HOW I do that import, so long as the code is readable.

The part that confuses most folks at first is the whole `if __name__ == '__main__'` statement. Python doesn't have much 
**boilerplate** - code that must be used pretty universally with little to no modification - but this is one of those 
rare bits.

`__name__` is a special string attribute of every Python module. If I were to stick the line `print(__name__)` at the top 
of `examplepackage/model/nice_model.py`, when that module got imported (and thus run), we'd see 
"examplepackage.model.nice_model" printed out.

When a module is run directly via `python -m some_module`, that module is assigned a special value of `__name__`: 
"**`__main__`**".

Thus, if `__name__ == '__main__':` is actually checking if the module is being executed as the _main_ module. If it is, 
it runs the code under the conditional.

You can see this in action another way. If I added the following to the bottom of `app.py` ...

```python
if __name__ == '__main__':
    run()
```

...I can then execute that module directly via `python -m examplepackage.app`, and the results are the same as 
`python -m examplepackage`. Now `__main__.py` is being ignored altogether, and the `__name__` of `examplepackage/app.py` is 
`"__main__.py"`.

Meanwhile, if I just run `python -m examplepackage`, that special code in `app.py` is ignored, since its `__name__` is now 
`examplepackage.app` again.
