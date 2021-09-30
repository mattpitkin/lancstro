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
|   ├── members/
|   |   ├── __init__.py
|   |   └── staff.py
|   └── data/
|       └── office_numbers.txt
└── bin/
    └── favourite_object.py
```

Here, there is a "submodule" called `members` within the main `lancstro` package.

### Using Github

Your package *should* be in a [version control](https://en.wikipedia.org/wiki/Version_control)
system and ideally hosted somewhere that provides a backup. It is now very common to use
[git](https://git-scm.com/about) for version control and it is sensible to host the project on
[Github](https://github.com/)/[Gitlab](https://about.gitlab.com/)/[bitbucket](https://bitbucket.org/product/)
or similar. On Github you can have public or private repositories.

If using Github, it is best to start the project by creating new repository there first and then
cloning that repository to you machine before then adding in your code. When creating a Github
repository (I might use "_repo_" for short later) you can initialise it with a [license
file](#the-license-file) and a [README file](#the-readmemd-file).

> Note: this is not a tutorial on using git, so you'll have to find that
> [elsewhere](https://git-scm.com/docs/gittutorial).

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

### The setup.cfg and setup.py files

In many packages you might just see a `setup.py` file, which is the build script used by setuptools.
However, it is now good practice to put ["static"
metadata](https://packaging.python.org/tutorials/packaging-projects/#configuring-metadata) about
your package in the `setup.cfg` [configuration
file](https://en.wikipedia.org/wiki/Configuration_file#Unix_and_Unix-like_operating_systems). By
"static" I mean any package information that does not have to be dynamically defined during the
build process (such as defining and building Cython
[extensions](https://setuptools.pypa.io/en/latest/userguide/extension.html)). In many cases, like
this repository, this can mean the [`setup.py`](setup.py) file can be very simple and just contain:

```python
from setuptools import setup

setup()
```

> Note: if using pip version greater than 19 for installing code, and/or if you're package contains
> a `pyproject.toml` file that specifies `setuptools>=40.9.0`, you don't actually
> [need](https://setuptools.pypa.io/en/latest/setuptools.html#setup-cfg-only-projects) the
> `setup.py` and you can just use the `setup.cfg` file.

The layout of the configuration file is described
[here](https://setuptools.pypa.io/en/latest/userguide/declarative_config.html), and I'll reproduce
the [one from this project](setup.cfg) below with additional inline comments:

```
[metadata]
# the name of the package
name = lancstro

# the package author information (multiple authors can just be separated by commas)
author = Matthew Pitkin
author_email = m.pitkin@lancaster.ac.uk

# a brief description of the package
description = Package defining the Lancaster Observational Astronomy group

# the license type and license file
license = MIT
license_files = LICENSE

# a more in-depth description of the project that will appear on it's PyPI page,
# in this case read in from the README.md file
long_description = file: README.md
long_description_content_type = text/markdown

# the projects URL (often the Github repo URL)
url = https://github.com/mattpitkin/lancstro

# standard classifiers giving some information about the project
classifiers =
    Intended Audience :: Science/Research
    License :: OSI Approved :: MIT License
    Natural Language :: English
    Programming Language :: Python
    Programming Language :: Python :: 3
    Programming Language :: Python :: 3.6
    Programming Language :: Python :: 3.7
    Programming Language :: Python :: 3.8
    Programming Language :: Python :: 3.9
    Topic :: Scientific/Engineering
    Topic :: Scientific/Engineering :: Astronomy
    Topic :: Scientific/Engineering :: Physics

# the package's current version (this isn't actually in the file, see later!)
version = 0.0.1

[options]
# state the Python versions that the package requires/supports
python_requires = >=3.6

# state packages and versions (of necessary) required for running the setup
setup_requires =
    setuptools >= 43
    wheel

# state packages and versions (if necessary) required for installing and using the package
install_requires =
    astropy
    astroquery >= 0.4.3

# automatically find all modules within this package
packages = find:

# include data in the package defined below
include_package_data = True

# any executable scripts to include in the package
scripts =
    bin/favourite_object.py

[options.package_data]
# any data files to include in the package (lancsrto shows they are in the
# lancstro packge and then the paths are given)
lancstro = 
    data/office_numbers.txt

```

For a list of the standard "classifiers" that you can add see [here](https://pypi.org/classifiers/).

In this project we have added some "data" files that come bundled with the package. It is not
required to include data in your package.

#### Adding a package version

In the above case the package version is set manually in the `setup.cfg` file. It is up to you how
you define the version string, but it is often good to used [Semantic
Versioning](https://semver.org/). In this format the version consists of three full-stop separated
numbers: MAJOR.MINOR.PATCH.

The Semantic Versioning site gives the following definitions of when to change the numbers:

> 1. MAJOR version when you make incompatible API changes,
> 2. MINOR version when you add functionality in a backwards compatible manner, and
> 3. PATCH version when you make backwards compatible bug fixes.
>
> Additional labels for pre-release and build metadata are available as extensions to the
> MAJOR.MINOR.PATCH format.

To update the version you can just edit the value in the `setup.cfg` file. When you
[install](#installing-the-package) this will be the packages version.

This allows the package manager (e.g., pip) to know what version of the package is installed.
However, it is often useful to provide the version number as a variable within the package itself,
so that the user can check it if necessary. Most often you will find this as a variable called
`__version__`, e.g.,:

```python
import numpy
print(numpy.__version__)
1.21.2
```

There are several ways to set this, but it is best to make sure that there's only one place that you
have to edit the version number rather than multiple places. One method (used in this package) is to
include the version number in your packages main `__init__.py` file by adding the line:

```python
__version__ = "0.0.1"
```

Then, within `setup.cfg`, the `version` line can be:

```
version = attr: lancstro.__version__
```

Among the other options, a good one to use is through setting the version with a tools such as
[`setuptools-scm`](https://pypi.org/project/setuptools-scm/), which gathers the version information
from [git tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging) in your repo.

#### The MANIFEST.in file

You can specify which additional files that you want to be bundled with the package's source
distribution using a [`MANIFEST.in`](https://packaging.python.org/guides/using-manifest-in/). With
modern versions of setuptools (e.g., greater than 43) most of the standard file such as the README
file and setup files, and any license file given in `setup.cfg`, are automatically included in the
source distribution by default.

However, you may want to include other files. If you had, say, a `test` directory with multiple
Python test scripts that you want in the package, you could add and `MANIFEST.in` file containing:

```
recursive-include test/ *.py
```

which will include all `.py` file within `test`.

### Installing the package

Once you have the above structure you can install the package (from it's base directory) using:

```bash
pip install .
```

That's it! Open up a Python terminal (from any location except in the package directory, otherwise
it'll get confused!) and you should be able to do:

```python
import lancstro
print(lancstro.__version__)
0.0.1
```

or run the [`favourite_object.py`](bin/favourite_object.py) script from the command line:

```console
$ favourite_object.py -h
usage: favourite_object.py [-h] name name

Get a staff member's favourite object

positional arguments:
  name        The staff member's full name

optional arguments:
  -h, --help  show this help message and exit
```

You can then tell other people to clone your Github repo and install things in the same way, or even
`pip install` directly from the repo with, e.g.:

```bash
$ pip install git+git://github.com/mattpitkin/lancstro.git#egg=lancstro
```

These will install the very latest code from the repo, so not necessarily a specific version
(although that can be done if you've tagged a version or supply the git hash).

### Releasing the package on PyPI

Rather than getting people to install code directly from your Github repo, it is often better to
publish versioned releases of your code. You can publish Python packages on the
[PyPI](https://pypi.org/) (Python Package Index) repository from which they will then be `pip
installable` by anyone!

Firstly, you'll need to [register an account](https://pypi.org/account/register/) on PyPI. Anyone is
able to do this. Secondly, you'll need to install the
[`twine`](https://twine.readthedocs.io/en/latest/) package, which is used for uploading packages to
PyPI.

Within your repo's root directory (containing `setup.py`) you can now build a [Python
wheel](https://pythonwheels.com/) (a zipped binary format of the package designed for speedier
installation) containing your package with:

```bash
python setup.py bdist_wheel
```

> Note: if your code is pure Python, creating a wheel should work straightforwardly, but if not the
> wheel generation may not work. In these cases you can just build a tarball containing the package
> using:
>
> ```bash
> python setup.py sdist
> ```

This should create a `dist/` directory containing a file with the extension `.whl`. This is the
Python wheel.

It is often best to first upload this to [PyPI's testing
repository](https://packaging.python.org/guides/using-testpypi/) (you'll need to [register a
separate account](https://test.pypi.org/account/register/) for this unfortunately), which can be
done using `twine` with:

```bash
twine upload -r testpypi dist/*
```

You should be prompted for your username and password, although there are ways to set these as
[environment variables](https://twine.readthedocs.io/en/latest/#environment-variables) or
[using](https://twine.readthedocs.io/en/latest/#keyring-support)
[`keyring`](https://pypi.org/project/keyring/), so that you don't have to enter them each time. If
the upload is successful you should be able to see the project on the Test PyPI site, e.g., at
https://test.pypi.org/project/lancstro/0.0.1/.

You can test that the package installs correctly from the Test PyPI repository by running
(potentially in a new virtual environment):

```bash
pip install -i https://test.pypi.org/simple/ lancstro
```

If you're happy with the package you can proceed to upload it to the main PyPI repository using:

```bash
twine upload dist/*
```

Et voilà! Now you just need to tell people to run:

```bash
pip install lancstro
```

to install your package. If they want to install a particular version they can use, e.g.,:

```bash
pip install lancstro==0.0.1
```

Or, if there's a lower or upper version that must be used the inequaltiy operators can be used instead, e.g.,:

```bash
pip install lancstro<=0.0.2
```

## Documentation

## Not covered here!

There are many additional useful things that I've not covered here. These include:

* using extry point console scripts rather than, or as well as, including executable scripts
* including C/C++/FORTRAN code, or [Cython](https://cython.org/)-ized code, in your package
* creating a [test suite](https://docs.pytest.org/) for your package (and checking its
  [coverage](https://pytest-cov.readthedocs.io/en/latest/))
* setting up [continuous integration](https://en.wikipedia.org/wiki/Continuous_integration) for
  building and testing (and automatically publishing) your code (e.g., with [Github
  Actions](https://docs.github.com/en/actions), TravisCI, ...)

I may add these at a later date.

## Other resources

For other descriptions of creating your Python code see:

* [Packaging Python Projects](https://packaging.python.org/tutorials/packaging-projects/)
* [`setuptools` documentation](https://setuptools.pypa.io/en/latest/index.html)
* ["How to package your Python code"](https://python-packaging.readthedocs.io/en/latest/index.html)
