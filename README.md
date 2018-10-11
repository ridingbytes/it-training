# IT/Dev Training

This repository provides a complete buildout environment for SENAITE.


## Quickstart

Read the [Setup SENAITE for development](docs/development.md) documentation how
to setup a virtual Python enviroment using [Conda](https://conda.io/miniconda.html).

Execute the following commands from within this directory:

```sh
$ python bootstrap.py
$ bin/buildout
```

After the build finished successfully, you can start the Zope instance using

```sh
$ bin/instance fg
```

The system can be accessed on http://localhost:8080 as soon as the line
```
2018-10-12 00:04:12 INFO Zope Ready to handle requests
```
was shown in the terminal.


