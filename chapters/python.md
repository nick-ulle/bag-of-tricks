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
want to import are in `../src/`. One way to solve this is to design `src/` as a
package and add this code to a cell at the beginning of each notebook, so that
Python searches `..` when something is imported:

```python
# Allow imports from `..`
import os
import sys

module_path = os.path.abspath("..")
if module_path not in sys.path:
    sys.path.append(module_path)
```

Then `import src` in the notebooks.

You can also run scripts from within the package with `python -m`.

Paired Notebooks
----------------

Jupyter notebooks can be stored in Markdown format (`.md`) via [Jupytext][].
This makes it easier to see changes to the notebooks in version control and
also avoids committing large images to the repo.

[Jupytext]: https://jupytext.readthedocs.io/en/latest/

Whenever you create a new Jupyter notebook, say `notebook.ipynb`, run this
command to make it a **paired notebook** and generate a corresponding
`notebook.md` file:

```sh
jupytext --set-formats 'ipynb,md' notebook.ipynb
```

The `notebook.md` file is the one to commit. Once a notebook is paired, the two
files will automatically be kept in sync as long as you run Jupyter in an
environment that has Jupytext installed.

If you over need to manually sync a paired notebook, the command is:

```sh
jupytext --sync notebook.ipynb
```
Be careful when doing this, because if you've somehow ended up with changes to
both `notebook.ipynb` and `notebook.md`, the older changes will be overwritten!

See the [Jupytext CLI docs][jupytext-cli] for more info.

[jupytext-cli]: https://jupytext.readthedocs.io/en/latest/using-cli.html


Pandas
------

Pandas has three different methods for [functional mapping][map] and I mix them
up. So:


* [`.map`][pd.map] maps over the elements of a `Series`. The argument can be a
  `dict` or `Series` lookup table instead of a function.
* [`.applymap`][pd.applymap] maps over the elements of a `DataFrame`.
* [`.apply`][pd.apply] maps over the elements of a `Series` or the rows or
  columns of `DataFrame`. The result can have a different type than the calling
  object (for example, when aggregating). 

Also see [this Stack Overflow post][so-pandas-map].

[map]: https://en.wikipedia.org/wiki/Map_(higher-order_function)
[pd.map]: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.map.html
[pd.applymap]: https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.applymap.html
[pd.apply]: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.apply.html
[so-pandas-map]: https://stackoverflow.com/questions/19798153/difference-between-map-applymap-and-apply-methods-in-pandas
