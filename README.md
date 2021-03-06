# Carneades sample code

This is a pedagogically-oriented attempt to implement aspects of the
[Carneades Argument Evaluation
framework](http://carneades.github.io/carneades/Carneades/). It
closely follows the Haskell implementation in the [CarneadesDSL
package](https://hackage.haskell.org/package/CarneadesDSL).

The libraries needed to run this code on DICE are already available
(except that the plotting functionality is currently not supported).
Just remember that you need to explicitly call ``python3.4`` since the
``python`` command on DICE defaults to ``python2.7``. So if you are
happy to work on DICE, you are ready to go. :smile:



If you want to install the necessary libraries on your own machine,
however, read on.

## Installing the libraries for the Carneades sample code on your own computer

### Requirements

* Python3.4
* igraph
* pycairo (for igraph)
* Virtualenv (Optional)
* Sphinx (docs only)
* Basicstrap theme for sphinx (docs only)

### Installation

#### Install `python3.4` (via apt, homebrew or your favourite package_system)

Note for Linux users (especially Ubuntu 12.04 users): follow this
[link](#install-python34) to have more info about installing the
python distro.

#### Create a virtualenv:

```bash
$ virtualenv -p python3.4 envname
$ source /envname/bin/activate
```

#### Install sphinx and basicstrap

```bash
$ pip install sphinx
$ pip install sphinxjp.themes.basicstrap
```

#### Install igraph

```bash
$ pip install python-igraph
```

This should also install the C bindings. If that doesn't happen
because of some error, it's likely to be one of the following
problems:

* The header files for the python distribution are not installed. Look
  up `build-essential`, and `python-dev` or `python3.4-dev`

```bash
# in a new terminal:
$ sudo apt-get install python-dev python3.4-dev
$ sudo apt-get install build-essential
```

* To install the C bindings:

``` bash
$ sudo apt-get install libxml2-dev
# download the igraph c bindings and extract.
$ wget http://igraph.org/nightly/get/c/igraph-0.7.1.tar.gz
$ tar xvf igraph-0.7.1.tar.gz
$ cd igraph-0.7.1

# Installation
$ ./configure
$ make
$ make install
$ sudo ldconfig # this does some magic!
```

#### Install pycairo

On Ubuntu, check this [package](http://packages.ubuntu.com/search?keywords=python-cairo).<br>
_By default this will install the package into `python` (which may not be `python3.4`)_ <br>
_(TODO: will this still work??))_

If you are using a virtualenv or you don't use Ubuntu, do the following:

* Download and extract the `pycairo` package _in_ the virtualenv

```bash
$ wget https://www.cairographics.org/releases/pycairo-1.10.0.tar.bz2
$ bunzip2 pycairo-1.10.0.tar.bz2
$ tar xvf pycairo-1.10.0.tar
$ cd pycairo-1.10.0/
$ ./waf configure # This will fail, but need to generate the hidden .waf folder first for below
```

* edit a file in the hidden .waf folder that the extraction has created (e.g. `./.waf-1.6.4-e3c1e08604b18a10567cfcd2d02eb6e6`. To do this, go into the folder and edit `./.waf-1.6.4-somenumbers/waflib/Tools/python.py` and replace the following line (see -/+ in codeblock below) to call the python3.4-config directly.
```diff
    --- waflib/Tools/python.py.old  2014-08-01 14:36:23.750613874 +0000
    +++ waflib/Tools/python.py      2014-08-01 14:36:38.359627761 +0000
    @@ -169,7 +169,7 @@
                    conf.find_program('python-config-%s'%num,var='PYTHON_CONFIG',mandatory=False)
            includes=[]
            if conf.env.PYTHON_CONFIG:
    -               for incstr in conf.cmd_and_log(conf.env.PYTHON+[conf.env.PYTHON_CONFIG,'--includes']).strip().split():
    +               for incstr in conf.cmd_and_log([conf.env.PYTHON_CONFIG,'--includes']).strip().split():
                            if(incstr.startswith('-I')or incstr.startswith('/I')):
                                    incstr=incstr[2:]
                            if incstr not in includes:
```

* compile and install the package

```bash
$ ./waf configure --prefix=$VIRTUAL_ENV
$ ./waf build
$ ./waf install
```
* If you run into 'cairo >= 1.10.0' error, you will need to install `cairo` and `pixman`:


Download and Install `pixman`
```bash
# Open a new terminal and run these commands:
$ wget https://www.cairographics.org/releases/pixman-0.34.0.tar.gz
$ tar xvf pixman-0.34.0.tar.gz
$ cd pixman-0.34.0

# configure make install:
$ ./configure
$ make
$ make install
```

Download and Install `cairo`
```bash
# continuing from above:
$ cd ..
$ wget https://www.cairographics.org/releases/cairo-1.14.6.tar.xz
$ tar xvf cairo-1.14.6.tar.xz
$ cd cairo-1.14.6

# configure make install:
$ ./configure
$ make
$ make install
```

You should now have both `cairo` and `pixman` installed in your machine. Return to installation for pycairo[link](#install-pycairo).


### Clone repository

```bash
$ git clone https://github.com/ewan-klein/carneades.git
$ cd carneades
```

#### Generate the documentation

```bash
$ cd doc
$ make html
```

#### Test the software

```bash
$ cd ../src/carneades # relative root is carneades/
$ python -i caes.py
```

### Install python3.4

(Linux users)
-------------

The following is to install python3.4 on Ubuntu 12.04 LTS. Other
distributions should do more or less the same. If you have a newer
installation of Ubuntu or derivatives, you can probably skip some
passages.

*WARNING: Although unlikely, this might break your python2 installation,
so do it at your own risk or set up a VM*

Unless you want to build from source (In which case, kudos!), add
[this PPA](https://launchpad.net/~fkrull/+archive/ubuntu/deadsnakes)
to your apt source list:

```bash
$ add-apt-repository ppa:fkrull/deadsnakes
$ apt-get update
```

then run:

```bash
$ apt-get install python3.4 python3.4-dev
```

Now follow the rest of the installation.
