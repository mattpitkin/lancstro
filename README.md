# Lancstro: an example of creating a Python package

This repository and the following text is intended as a basic tutorial on creating and publishing a
Python package. It was created for a seminar given to the Lancaster University [Obervational
Astrophysics
group](https://www.lancaster.ac.uk/physics/research/astrophysics/observational-astrophysics/), but
may be more widely applicable.

## What is a Python package

In general, when talking about a Python package it means an set of Python modules and/or scipts
and/or data, that are installable under a common namespace (the package's name). A package might
also be referred to as a library. This is different than a collection of individual Python files
that you have in a folder, which will not be under a common namespace and are only accessible if
their path is in your `PYTHONPATH` or you use them from the directory in which they live.

A couple of examples of common Python packages used in research in the physical sciences are:

1. NumPy
2. SciPy

> Note: "namespace" basically refers to the name of the package as you would import it, e.g., if you
> import numpy with `import numpy`, then you will access all NumPy's functions/classes/modules via
> the `numpy` namespace:
> ```python
> numpy.sin(2.3)
> ```

A package can contain everything within it's namespace, or contain various submodules, e.g., parts
that contain common functionality that naturally fits together in it's own namespace. For example,
in NumPy, the [`random`](https://numpy.org/doc/stable/reference/random/index.html) submodule
contains functions and classes for generating random numbers:

```python
import numpy
numpy.random.randn()  # generate a normally distributed random number
```

## Why package my code?

So, why should you package your Python code rather than just having local scripts? Well, there are
several reasons:

* It creates an installable package that can be imported without having to have the Python
  script/file in your path.
* It creates a “versioned” package that can have specified features/dependencies.
* You can share you package with others (you can make it `pip installable` via
  [PyPI](https://pypi.org/), or `conda installable` via [conda-forge](https://conda-forge.org/))
* You will gain developer kudos! Software development is a major skill you learn during your
  research, so show off what you’ve done and add it to your CV.

## Project structure

To create a Python package you should structure the directory containing you code in the following way
(the directory name containing this information does not have to match the package name):

```
repo/
├── LICENSE
├── pyproject.toml
├── README.md
├── setup.cfg
├── setup.py
├── pkgname/
│   ├── __init__.py
│   └── example.py
└── bin/
    └── executable_script.py
```

There are other slight variations on this, for example, using a `src` directory in which your
package directories live, as described in the [official
guidelines](https://packaging.python.org/tutorials/packaging-projects/#creating-the-package-files)).

In this project the structure is:

```
lancstro/
├── LICENSE
├── pyproject.toml
├── README.md
├── setup.cfg
├── setup.py
├── lancstro/
│   ├── __init__.py
│   ├── base.py.py
|   └── members/
|       ├── __init__.py
|       └── staff.py
└── bin/
    └── favourite_object.py
```

Here, there is a "submodule" called `members` within the main `lancstro` package.

### The LICENSE file

You should give your code a license describing the terms of use and copyright. Often you'll want
your code to be open source, so a good choice is the [MIT
license](https://opensource.org/licenses/MIT), which is very permissive in terms of reuse of your
code. A [variety of other open source licenses](https://opensource.org/licenses/category) are
available, although these often differ slighty on the permissiveness, i.e., whether others can use
your code in commercial and non-open source projects or not.

The `LICENSE` file will contain a plain ascii text copy of your license.

### The pyproject.toml file

This file tells the [`pip`](https://packaging.python.org/key_projects/#pip) tool used for installing
packages how it should build the package. In this repo we have used the [file
contents](pyproject.toml) suggested
[here](https://packaging.python.org/tutorials/packaging-projects/#creating-pyproject-toml), which
means that the [`setuptools`](https://setuptools.pypa.io/en/latest/index.html) package is used for
the build.

### The README.md file

This is the file that you are currently reading! It should provide a basic description of your
package, maybe including information about how to install it. Ideally it should be brief and not be
seen as a replacement for having proper [documentation](#documentation) for you code available
elsewhere.

In this case the suggested format for the file is
[Markdown](https://daringfireball.net/projects/markdown/) (the `.md` extension), but it could be a
plain ascii text file for [reStructedText](https://docutils.sourceforge.io/rst.html). Markdown and
reStructuredText will be automatically rendered if you host your package on, e.g.,
[Github](https://github.com/).

### Using Github

Your package *should* be in a version control system. It is sensible to host it on
[Github](https://github.com/)/[Gitlab](https://about.gitlab.com/)/[bitbucket](https://bitbucket.org/product/)
or similar. On Github you can have public or private repositories.

## Documentation

## Other resources

For other descriptions of creating your Python code see:

* [Packaging Python Projects](https://packaging.python.org/tutorials/packaging-projects/)
* [`setuptools` documentation](https://setuptools.pypa.io/en/latest/index.html)
* ["How to package your Python code"](https://python-packaging.readthedocs.io/en/latest/index.html)
