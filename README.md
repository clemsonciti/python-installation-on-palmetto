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

$ python
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


## The Anaconda modules

## Using `pip` to install Python packages

## Using `conda` to manage Python environments
