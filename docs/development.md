# Setup SENAITE for development

This document describes the steps how to prepare your system for SENAITE
development.


## SENAITE Dependencies

SENAITE depends on the following systems:

- Plone 4.x: https://plone.org (currently 4.3.18)
- Python 2.x: https://www.python.org (currently 2.7.15)


### Python

Most UNIX based operating system (Linux/Mac OSX) ship already with a Python
interpreter installed. However, it is not recommended to use the system
interpreter to setup and install SENAITE on the local system.

Besides the required super-user permissions for installing additional Python
libraries is that it might get upgraded by the system and get incompatible.

Therefore, it is better to setup a virtual Python environment with one of the
following tools:

- Virtualenv: https://pypi.org/project/virtualenv
- Miniconda: https://conda.io/miniconda.html

In this manual we will use Miniconda.

### Miniconda

Please use your terminal to run the commands listed below.

1. Download the `Python 2.7` version for your operating system
2. Create a virtual environment with `conda create --name senaite python=2.7`
3. Activate the environment with `conda activate senaite`

The command `which python` can be used to check if the right Python interpreter
is active in the current shell:

```sh
$ which python
~/miniconda2/envs/senaite/bin/python
$ python
Python 2.7.15 |Anaconda, Inc.| (default, May  1 2018, 18:37:05)
[GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>>
```

The *Conda Cheat Sheet* helps to find the most common `conda` commands quickly:

https://conda.io/docs/_downloads/conda-cheatsheet.pdf


## Buildout

Buildout is a tool to build a self contained software environment. It uses the
[INI syntax](https://en.wikipedia.org/wiki/INI_file) for configuration and is
therefore arranged in `[sections]` followed by option `name=value` pairs.

The minimum SENAITE `buildout.cfg` looks like this::

```ini
[buildout]
extends = http://dist.plone.org/release/4.3.18/versions.cfg

index = https://pypi.python.org/simple/

find-links =
    http://dist.plone.org/release/4.3.18
    http://dist.plone.org/thirdparty

parts =
    instance

[instance]
recipe = plone.recipe.zope2instance
user = admin:admin
eggs =
    Plone
    Pillow
    senaite.core
    senaite.lims
```

To install `buildout` command can be obtained by the `bootstrap.py` script or
by installing `zc.buildout` via `pip`.

We will download here the `bootstrap.py` script to the same directory where
the configuration `buildout.cfg` is stored to initiate the development environment:

```sh
$ wget https://raw.githubusercontent.com/buildout/buildout/master/bootstrap/bootstrap.py
$ python bootstrap.py
```

This installs the `buildout` command in the `bin` folder.
The directory structure should look similar to this afterwards:

```sh
$ tree -L 2
.
├── bin
│   └── buildout
├── bootstrap.py
├── buildout.cfg
├── develop-eggs
├── eggs
│   ├── setuptools-33.1.1-py2.7.egg
│   └── zc.buildout-2.12.2-py2.7.egg
└── parts
```

Run `bin/buildout -c buildout.cfg` to initiate the build process.

Common buildout configurations for SENAITE development can be found here:

- SENAITE CORE buildout.cfg:
  https://github.com/senaite/senaite.core/blob/master/buildout.cfg

- SENAITE LIMS:
  https://github.com/senaite/senaite.lims/blob/master/buildout.cfg

- SENAITE IMPRESS:
  https://github.com/senaite/senaite.impress/blob/master/buildout.cfg

- SENAITE DATABOX:
  https://github.com/senaite/senaite.databox/blob/master/buildout.cfg
