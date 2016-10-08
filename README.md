<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Managing Python Installations on the Palmetto Cluster](#managing-python-installations-on-the-palmetto-cluster)
  - [Python versions available on Palmetto Cluster](#python-versions-available-on-palmetto-cluster)
    - [The default (system) Python](#the-default-system-python)
    - [Python modules](#python-modules)
  - [Anaconda modules](#anaconda-modules)
  - [Installing Python packages](#installing-python-packages)
    - [Using pip to install packages and dependencies](#using-pip-to-install-packages-and-dependencies)
    - [Building the package yourself](#building-the-package-yourself)
  - [Using `conda` to manage Python environments](#using-conda-to-manage-python-environments)
    - [Creating an environment](#creating-an-environment)
    - [Installing packages in an environment](#installing-packages-in-an-environment)
    - [Cloning an existing environment](#cloning-an-existing-environment)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# Managing Python Installations on the Palmetto Cluster

This article is for anyone using Python and Python packages
on the Palmetto cluster.
It aims to explain the different options
for installing Python and Python packages on the cluster.
As an example,
consider the scenario of a user working on two Python projects,
each project has different requirements:

<img src="img/python-envs.eps" width=200px">

This article will aim to explain

- how to use tools such as `pip` to install Python packages
(and automatically install their dependencies),

- how to leverage Python distributions like "Anaconda"
which come with several important Python packages
pre-installed, thus removing the burden of manually installing them.

- how to use the `conda` package manager to easily create
isolated Python installations - each with its own version
of Python, and its own set of packages.
This is the most reliable and efficient way to manage
Python installations,
and users are encouraged to adopt this approach.

## Python versions available on Palmetto Cluster

One of the first points of confusion
for many users may be about
the different versions of Python available on the Palmetto cluster.
Currently,
the versions of Python used by the community
are either **2.X.Y** (for example, 2.7.6)
or **3.X.Y** (for example, 3.5.2).
The Palmetto cluster makes many versions of Python available,
and we will begin by enumerating them:

### The default (system) Python

The "default" version of Python on the Palmetto cluster
(also known as the *system* Python),
is **2.6.6**. This is the version of Python that
ships with the operating system running on Palmetto
(currently Scientific Linux release 6.7).

```bash
$ module list
No Modulefiles Currently Loaded.

$ which python
/usr/bin/python

$ python --version
Python 2.6.6
```

In general,
it is highly recommended that users
**not** rely on this version of Python to
run their own applications:

1. Libraries that they use may require other versions
of Python (generally, 2.7 or higher).

2. The system version of Python may change.
Any applications or packages that users
may build using the system Python
will likely not run if this happens.

### Python modules

In addition to the default system Python,
the Palmetto cluster enables users to
load different versions of Python as modules:

```bash
$ module avail python

------------------------- /software/modulefiles -------------------------
python/2.7.6 python/3.3.3 python/3.4
```

When any of these modules is added,
the version of Python used changes.
For example, here is the effect of adding
the `python/3.4` module:

```bash
$ module list
No Modulefiles Currently Loaded.

$ python --version
Python 2.6.6

$ module add python/3.4

$ python --version
Python 3.4.2

$ which python
/software/python/3.4/bin/python
```

Using modules
allows users to easily change the
version of Python from the system Python.
With modules,
users can install packages and applications
that depend on different versions of Python than 2.6.6.
Users can also maintain packages
separately for Python 2 and Python 3;
for example,
a version of the `numpy` package  for Python 2,
and another version of `numpy` package for Python 3.

## Anaconda modules

There are also several "Anaconda" modules
available on the cluster.
Loading one of the "Anaconda" modules
also loads a different version of Python:

```bash
$ module avail anaconda
anaconda/1.9.1  anaconda/2.3.0  anaconda/2.4.0  anaconda/2.5.0  anaconda/4.0.0  anaconda3/2.5.0 anaconda3/4.0.0

$ module add anaconda3/2.5.0

$ python --version
Python 3.5.2 :: Anaconda 2.5.0 (64-bit)

$ which python
/software/anaconda3/2.5.0/bin/python
```

[Anaconda][anaconda-overview] is a Python "distribution",
which bundles together several Python (and non-Python) packages
that are used in scientific computing and data analysis,
including `numpy`, `scipy`, `matplotlib`, `pandas`, and
several hundreds of others.
Some of these packages, such as
`numpy`, or [PyTables](http://www.pytables.org/usersguide/installation.html) for example,
have numerous dependencies, making them difficult to install from source.
Using the Anaconda distribution removes the burden
of manually installing these packages from source.
Once the Anaconda module is loaded,
importing these packages "just works":

```bash
$ module add anaconda3/2.5.0
$ python
Python 3.5.2 |Anaconda 2.5.0 (64-bit)| (default, Jul  2 2016, 17:53:06)
[GCC 4.4.7 20120313 (Red Hat 4.4.7-1)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import tables
>>> tables.__file__     # prints where the tables module is loaded from
'/software/anaconda3/2.5.0/lib/python3.5/site-packages/tables/__init__.py'
```

Importantly, Anaconda provides the `conda` package manager,
which will be discussed later in this article.

## Installing Python packages

While the provided Python and Anaconda modules
have several important Python packages installed and ready
for importing,
you will often find that you may need to install additional packages,
or different versions of already available packages.
How you install a Python package depends
on how the package is distributed,
and what options it provides for installation.

A large number of packages can be installed in one of the following ways
on the Palmetto cluster:

1. Using the `pip` package manager to install/upgrade the package and its dependencies automatically
2. Building the package and its dependencies yourself
3. Using the `conda` package manager to install/upgrade the package and its dependencies automatically

Several packages will provide the option
to install in more than one way.
For example, see the [installation instructions
for the `mpi4py` package][mpi4py-install],
which can be installed either using `pip`,
or by downloading and building from source.

It is not necessary that you install *all* packages
using `pip`,
or build *all* packages yourself.
Your setup can contain several packages,
and each one may be installed in any of the above ways.

### Using pip to install packages and dependencies

To install a package using `pip`,
you should first have `pip` installed
for whatever version of Python you are running.
All the provided Python and Anaconda modules already come with `pip`.
If `pip` is installed,
you can install the package using the command:


```bash
$ pip install package-name --user
```

The `--user` switch ensures that the package is installed
into your home directory, and not into the `/usr/local` directory.

This will only install the package
for the currently running version of Python.

Whenever possible, you should try to use `pip`
to install Python packages,
as it correctly installs any Python dependencies
that the package may have.
Unfortunately,
`pip` is generally not well suited for installing scientific software packages
like `numpy` or `matplotlib`.
In this case,
the available options are to
build the package yourself,
use the `conda` package manager to install the package,
or to use a Python distribution like Anaconda,
which comes pre-installed with scientific and data analysis libraries.

### Building the package yourself

When packages cannot be installed via pip
or when you want a package to be built in a specific way,
e.g., linked against specific libraries,
you may need to build the package for yourself.

In general,
dependencies are *not* automatically installed
when you manually build a package.
Thus, you may need to ensure that the dependencies
are available before attempting to build a package.

The instructions for building a package are generally
a part of the package's documentation.
Many packages are built using the following two commands:

```
$ python setup.py build
$ python setup.py install --user
```

The `--user` switch ensures that the package is installed
into your home directory, and not into the `/usr/local` directory.
This will only install the package
for the currently running version of Python.

## Using `conda` to manage Python environments

`conda` is a package manager similar to `pip`,
but with two major improvements over `pip`:

1. `conda` also installs non:-Python packages and dependencies
such as `gcc`, MKL, or HDF5.

2. `conda` also serves 

### Creating an environment
### Installing packages in an environment
### Cloning an existing environment

[mpi4py-install]: https://mpi4py.scipy.org/docs/usrman/install.html
[python-packaging-science]: https://packaging.python.org/science/
[anaconda-overview]: https://www.continuum.io/anaconda-overview
