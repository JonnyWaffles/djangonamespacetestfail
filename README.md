## Problem Statement
Demonstrates packages within a namespace will fail to unit test
because the namespace ``namespace`` is provided as the argument module name to ``unittest.loader.TestLoader._get_directory_containing_module``
"module_name" parameter, which results in unittest assuming ``namespace`` is a module, and not a package, thus
the repository root directory's parent directory is set to ``self._top_level_dir`` in ``discover`` on line 337.

For example, if the repository root directory for this project is ``namespaceunittest`` the following error is thrown.
```cmd
(namespaceunittest) E:\users\jfuller\documents\PycharmProjects\namespaceunittest>python manage.py test namespace.project
System check identified no issues (0 silenced).
E
======================================================================
ERROR: namespaceunittest.namespace.project (unittest.loader._FailedTest)
----------------------------------------------------------------------
ImportError: Failed to import test module: namespaceunittest.namespace.project
Traceback (most recent call last):
  File "C:\Program Files\Python37\lib\unittest\loader.py", line 468, in _find_test_path
    package = self._get_module_from_name(name)
  File "C:\Program Files\Python37\lib\unittest\loader.py", line 375, in _get_module_from_name
    __import__(name)
ModuleNotFoundError: No module named 'namespaceunittest'


----------------------------------------------------------------------
Ran 1 test in 0.000s

FAILED (errors=1)
```
## To reproduce
Run
```cmd
python manage.py test namespace.project
```
Or any variation trying to run the test.
```cmd
python manage.py test namespace.project.app.tests
```
However, selecting the exact test module as unittest properly finds the __init\__.py.
```cmd
python manage.py test namespace.project.app.tests.test_namespace
```
Succeess
```cmd
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.
----------------------------------------------------------------------
Ran 1 test in 0.001s

OK
Destroying test database for alias 'default'...

```
