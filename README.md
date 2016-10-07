# Managing Python Installations on the Palmetto Cluster

This article is for anyone using Python and Python packages
on the Palmetto cluster.
It aims to explain the different options
for managing Python installations
on the Palmetto Cluster.
A "Python installation" is defined as:

* Some version of Python, and
* Python packages for this version

Note that, in this context, a Python "package"
is a bundle of software to be installed.
Python libraries such as `numpy`
are distributed as packages;
and the first step towards using these libraries
is to install the corresponding packages.

This also aims to clear up some common points of confusion,
such as:

* what is the difference between "Python" and "Anaconda",
* where Python looks for installed packages,
* what exactly is `pip`

## The default (system) Python

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

## The Python modules

In addition to the default, system Python,
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

## Installing Python packages

Running the correct version of Python is the first step
towards successfully running your application.
Installing the required packages (and their dependencies)
is the next step.
How you install a Python package depends
on how the package is distributed,
and what options it provides for installation.

A large number
packages can be installed in one of the following ways
on the Palmetto cluster:

1. Using `pip` to install the package and its dependencies automatically
3. Using `conda` to install the package and its dependencies automatically
2. Building the package and its dependencies yourself

Several packages will provide the option
to install in more than one way.
For example, see the [installation instructions
for the `mpi4py` package][mpi4py-install],
which can be installed either using `pip`,
or by downloading and building from source.

### Using pip

To install a package using `pip`,
simply run the following command:

```bash
$ pip install package-name --user
```

For example,
to install the [Cython](http://cython.org/) package using `pip`::

```bash
$ pip install cython --user

Collecting cython
  Downloading Cython-0.24.1-cp35-cp35m-manylinux1_x86_64.whl (6.5MB)
      100% |████████████████████████████████| 6.5MB 119kB/s
      Installing collected packages: cython
      Successfully installed cython-0.24.1
```

this installs the package in your home directory
(in `/home/username/.local/lib/pythonX.Y/site-packages`).
Here, `pythonX.Y` is the specific version of Python that is used
when performing the `pip install` (for example `python3.5`).
This installed version of Cython will only be used
by Python 3.5.
For any other version of Python (for examle, Python 2.7),
this version of Cython will *not* be used,
you will have to do a separate `pip install`.

Whenever possible, you should try to use `pip`
to install Python packages,
as it correctly installs any Python dependencies
that the package may have.
Unfortunately,
`pip` is generally not well suited for installing scientific software packages
like `numpy` or `matplotlib`.
This is because (as explained in [this][python-packaging-science] article),
"scientific software tends to have more complex dependencies than most,
and it will often have multiple build options to take advantage of different kinds of hardware,
or to interoperate with different pieces of external software."
This makes it difficult to distribute pre-built versions of NumPy,
as "most users aren’t going to know which version they need,
and current Python installation tools cannot make that decision on the
user’s behalf at install time."

In this case,
two available options are to
build the package yourself,
or to use the `conda` package manager to install the package.

### Building the package yourself

### Using conda

## The Anaconda modules

## Using `conda` to manage Python environments


[mpi4py-install]: https://mpi4py.scipy.org/docs/usrman/install.html
[python-packaging-science]: https://packaging.python.org/science/
