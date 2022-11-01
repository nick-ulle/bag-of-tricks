Python
======

This chapter is about [Python][python].

[python]: https://www.python.org/


Notebooks with Scripts
----------------------

My project repos typically have a `src/` directory for code and a `notebooks/`
directory for notebooks, both at the top level. The notebooks tend to contain
experiments and exploratory data analyses. When some processing code seems
reasonably figured-out and stable, I migrate the code into a Python script so
that I can easily reuse it wherever I need it.

By default, Python only searches for scripts to import in the startup
directory. For notebooks, that's `notebooks/`, and so most of the scripts I'd
want to import are in `../src/`. Adding this code to a cell at the beginning of
a notebook tells Python to also search `../src/` when something is imported:

```python
# Allow imports from `../src/`
import os
import sys

module_path = os.path.abspath(os.path.join("..", "src"))
if module_path not in sys.path:
    sys.path.append(module_path)
```
