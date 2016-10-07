# Managing Python Installations on the Palmetto Cluster

This article is for anyone using Python and Python libraries
on the Palmetto cluster.
It aims to explain the different options
for managing Python installations
on the Palmetto Cluster.
A "Python installation" is defined as:

* Some version of Python, and
* Python libraries for this version

It also aims to clear up some common points of confusion,
such as the difference between "Python" and "Anaconda",
and what exactly `pip install` does.

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
Any applications or libraries that users
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
users can install libraries and applications
that depend on different version of Python than 2.6.6.
Users can also maintain libraries
separately for Python 2 and Python 3,
for example,
a version of the `numpy` library for Python 2,
and another version of `numpy` for Python 3.

**Caution**: later in this article,
we will see how it is generally a bad idea for users
to try and compile Python libraries such as `numpy` by themselves.

## The Anaconda modules

## Using `pip` to install Python packages

## Using `conda` to manage Python environments
