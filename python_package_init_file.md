# Background
Python 3.3+ has implicit namespace packages that allow you to create packages without __init__.py. In Python 2, __init__.py is the old method and still works.

> _Note_: init.py files were used to mark directories on your disk as Python packages.

# Namespace packages

Namespace packages are a special kind of package that allows you to unify two packages with the same name at different points on your Python-path. For example, consider path1 and path2 as separate entries on your Python-path:

path1
+--namespace
      +--module1.py
      +--module2.py
path2
+--namespace
      +--module3.py
      +--module4.py

With this arrangement you should be able to do the following:

```
from namespace import module1, module3
```

thus you get the unification of two packages with the same name in a single namespace. If any of them have an __init__.py that one becomes the package - and you no longer get the unification as the other directory is ignored.

So it appears that the fundamental difference between a namespace package and a regular package, beside the obvious difference that a regular package may contain various initialization code in __init__.py, is that a namespace package is a virtual package whose contents can be distributed in various places along Python's lookup path.

For example, given:
```
a/foo/bar.py
b/foo/baz.py
```

If both b and a are in Python's path, you can import foo.bar and foo.baz freely.
