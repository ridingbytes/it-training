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

After the build finished successfully, a script in `bin/instance` is available
to control the Zope Server.

Start the Zope instance:

    $ bin/instance start

Stop the Zope instance:

    $ bin/instance stop

Start the Zope instance if foreground (recommended for development):

    $ bin/instance fg

If Zope was started in foreground, it can be accessed on http://localhost:8080 as soon as the line:

    2018-10-12 00:04:12 INFO Zope Ready to handle requests

was displayed in the terminal.

You can use `Ctrl+C` (Hold `Ctrl` on your keyboard and then press `C`) to stop
the instance again.
