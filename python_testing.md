# Choosing a test layout with pytest
## Tests as part of application code
Inlining test directories into your application package is useful if you have direct relation between tests 
and application modules and want to distribute them along with your application:

```bash
setup.py
mypkg/
    __init__.py
    app.py
    view.py
    test/
        __init__.py
        test_app.py
        test_view.py
        ...
```

## Tests outside application code (preferred)
Putting tests into an extra directory outside your actual application code might be useful if you have many 
functional tests or for other reasons want to keep tests separate from actual application code 
(often a good idea):

```bash
setup.py
mypkg/
    __init__.py
    app.py
    view.py
tests/
    test_app.py
    test_view.py
    ...
```

This has the following benefits:

- Your tests can run against an installed version after executing pip install xxx
- Your tests can run against the local copy with an editable install after executing 
pip install --editable .
- If you don’t have a setup.py file and are relying on the fact that Python by default puts the current 
directory in sys.path to import your package, you can execute python -m pytest to execute the tests against 
the local copy directly, without using pip

> Note that using this scheme your test files must have unique names, because pytest will import them as 
top-level modules since there are no packages to derive a full package name from. In other words, the test 
files in the example above will be imported as test_app and test_view top-level modules by adding tests/ 
to sys.path.

If you need to have test modules with the same name, you might add __init__.py files to your tests folder 
and subfolders, changing them to packages:

```bash
setup.py
mypkg/
    ...
tests/
    __init__.py
    foo/
        __init__.py
        test_view.py
    bar/
        __init__.py
        test_view.py
```

Now pytest will load the modules as tests.foo.test_view and tests.bar.test_view, allowing you to have 
modules with the same name. But now this introduces a subtle problem: in order to load the test modules from 
the tests directory, pytest prepends the root of the repository to sys.path, which adds the side-effect that 
now mypkg is also importable. This is problematic if you are using a tool like tox to test your package in a 
virtual environment, because you want to test the installed version of your package, not the local code from 
the repository.

In this situation, it is strongly suggested to use a src layout where application root package resides in a 
sub-directory of your root:

```bash
setup.py
src/
    mypkg/
        __init__.py
        app.py
        view.py
tests/
    __init__.py
    foo/
        __init__.py
        test_view.py
    bar/
        __init__.py
        test_view.py
```

### ModuleNotFoundError
When using the tests outside application code layout, you can get the error `ModuleNotFoundError`. The 
following workaround is possible as a temporary solution: 

Simply add the following file to your 'tests' directory, and then pytest will run it before the tests 
(so no changes are needed per-file when you make a final package, you just delete this one file):

`conftest.py`

```python
import os
import sys
sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))
```

Although the above is possible, it is not recommended. Always run tests against your installed package. If 
your package is not installable yet: **make it installable**. When you develop, install your package with 
`pip install --editable <path/to/package>` _(ex. pip install --editable .)_.

In most cases it is really much easier to create a simple `setup.py` to make your project installable and run 
tests against that, than going through all this hackery to make it work without.

Link: https://pytest.readthedocs.io/en/latest/goodpractices.html
