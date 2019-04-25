## Problem Statement
Demonstrates packages within a namespace will fail to unit test
because the namespace ``namespace`` is provided as the argument module name to ``unittest.loader.TestLoader._get_directory_containing_module``
"module_name" parameter, which results in unittest assuming ``namespace`` is a module, and not a package, thus
the repository root directory's parent directory is set to ``self._top_level_dir`` in ``discover`` on line 337.

For example, if the repository root directory for this project is ``namespaceunittest`` the following error is thrown.
#### Python 3.7 error message
```cmd
(namespaceunittest) E:\users\jfuller\documents\PycharmProjects\namespaceunittest>python manage.py test namespace.app
System check identified no issues (0 silenced).
E
======================================================================
ERROR: namespaceunittest.namespace.app (unittest.loader._FailedTest)
----------------------------------------------------------------------
ImportError: Failed to import test module: namespaceunittest.namespace.app
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
#### Python 3.6 error message
```cmd
(namespaceunittest36) E:\users\jfuller\documents\PycharmProjects\namespaceunittest>python manage.py test namespace.app
Traceback (most recent call last):
  File "manage.py", line 20, in <module>
    main()
  File "manage.py", line 16, in main
    execute_from_command_line(sys.argv)
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\core\management\__init__.py", line 381, in execute_from_command_line
    utility.execute()
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\core\management\__init__.py", line 375, in execute
    self.fetch_command(subcommand).run_from_argv(self.argv)
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\core\management\commands\test.py", line 23, in run_from_argv
    super().run_from_argv(argv)
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\core\management\base.py", line 323, in run_from_argv
    self.execute(*args, **cmd_options)
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\core\management\base.py", line 364, in execute
    output = self.handle(*args, **options)
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\core\management\commands\test.py", line 53, in handle
    failures = test_runner.run_tests(test_labels)
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\test\runner.py", line 627, in run_tests
    suite = self.build_suite(test_labels, extra_tests)
  File "E:\users\jfuller\documents\PycharmVENVs\namespaceunittest36\lib\site-packages\django\test\runner.py", line 517, in build_suite
    tests = self.test_loader.discover(start_dir=label, **kwargs)
  File "C:\Program Files\Python36\lib\unittest\loader.py", line 332, in discover
    self._get_directory_containing_module(top_part)
  File "C:\Program Files\Python36\lib\unittest\loader.py", line 346, in _get_directory_containing_module
    full_path = os.path.abspath(module.__file__)
AttributeError: module 'namespace' has no attribute '__file__'

```
## To reproduce
Run
```cmd
python manage.py test namespace.app
```
Or any variation trying to run the test.
```cmd
python manage.py test namespace.app.tests
```
However, selecting the exact test module as unittest properly finds the __init\__.py.
```cmd
python manage.py test namespace.app.tests.test_namespace
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
Additionally, testing the entire namespace works
```cmd
(namespaceunittest36) E:\users\jfuller\documents\PycharmProjects\namespaceunitte
st>python manage.py test namespace
Creating test database for alias 'default'...
System check identified no issues (0 silenced).
.
----------------------------------------------------------------------
Ran 1 test in 0.001s

OK
Destroying test database for alias 'default'...
```
