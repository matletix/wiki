+++
title = "Python Packaging"
date = "2020-06-01T22:51:08+01:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["python", "packaging", "setuptools"]
keywords = ["", ""]
description = "Installing the python package you’re developing allows you to import it by name, without using relative imports (in your tests for example), as it’ll be in the python path."
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

Installing the python package you’re developing allows you to import it by name,
without using relative imports (in your tests for example),
as it’ll be in the python path.

You can do so by creating a setup.py file at the root of your package and running
the following from the package’s root :

```bash
pip install -e .
```

This will create an `egg-info` folder in your package and an `.egg-link` file pointing to
your package code in the site-packages folder of your environment.
It will also add the path to your package in the `site-packages/easy-install.pth` file,
making it discoverable by the python interpreter.

Another way to modify Python’s search path is to add a path configuration file to a
directory that’s already on Python’s path, usually to the `site-packages/` directory.
Path configuration files have an extension of `.pth`, and each line must contain a
single path that will be appended to `sys.path`. Because the new paths are appended
to `sys.path`, modules in the added directories will not override standard modules.
Paths can be absolute or relative, in which case they’re relative to the directory
containing the `.pth` file.

A pytest-specific approach to modifying `sys.path` is to add an empty file, `conftest.py`,
to your project’s root directory. Pytest looks for files of that name and, on detecting
their presence, marks the containing directory as a project to be tested and adds the
directory to the Python path during the test run.

## Links
- https://www.tjelvarolsson.com/blog/begginers-guide-creating-clean-python-development-environments/
- https://packaging.python.org/tutorials/packaging-projects/
- https://docs.python.org/3/install/index.html#inst-search-path
- https://stackoverflow.com/questions/34466027/in-pytest-what-is-the-use-of-conftest-py-files