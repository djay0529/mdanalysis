# Installing MDAnalysis #

These instructions are for users of the package. Developers should also look at DevelopmentWorkflow.



## Installing using binary packages (Linux users) ##
As for **MDAnalysis** 0.8.0, binary packages are available for a few Linux distributions. These packages are hosted on the [Open Build Service](http://openbuildservice.org/) instance provided by the [OpenSUSE ](http://www.opensuse.org/) project.

The repository home page is here: https://build.opensuse.org/project/show/home:MDyNMR-FTW:MDAnalysis

Here is a summary on how to use the repository for the supported distribution.

### Debian & Ubuntu ###

On .deb-package-based distributions, like Debian and Ubuntu, you need to add the repository to your sources.list file (most probably located in /etc/apt). The description line for repo follows the following:
```
    deb http://download.opensuse.org/repositories/home:/MDyNMR-FTW:/MDAnalysis/[your_distro_code]  ./
```

Here are the following versions that are supported and their appropriate code:

  * Debian 7.0: Debian\_7.0
  * Ubuntu 12.04: xUbuntu\_12.04
  * Ubuntu 13.10: xUbuntu\_13.10

For example, on a Debian 7 system you should add the following line to your sources.list:
```
    deb http://download.opensuse.org/repositories/home:/MDyNMR-FTW:/MDAnalysis/Debian_7.0 ./
```

### Fedora ###

On Fedora 19, add the following repository to your system:
```
    http://download.opensuse.org/repositories/home:/MDyNMR-FTW:/MDAnalysis/Fedora_19/
```

On Fedora 20, add the following repository to your system:
```
    http://download.opensuse.org/repositories/home:/MDyNMR-FTW:/MDAnalysis/Fedora_20/
```


### OpenSUSE ###

You can install from the software database:
http://software.opensuse.org/package/python-MDAnalysis


## Installing from source ##
**MDAnalysis** is also distributed in source form. In order to build the library some C code needs to be compiled. Thus you will need
  * python (>=2.6)
  * a C compiler (GNU `gcc` works),
  * the Python header files (typically in a package `python-dev`),
  * [numpy](http://numpy.scipy.org) to compile the DCD reader and numerical extensions,

See
[Additional non-standard packages](http://code.google.com/p/mdanalysis/wiki/Install#Additional_non-standard_packages) below for what else you need at run
time.

The source code can be obtained via the [Downloads](Downloads.md) page and as described below under [Getting the source](#Getting_the_source.md). The primary dependency is [numpy](http://www.numpy.org/).

Please read through this whole document. Installing MDAnalysis is unfortunately not always absolutely straightforward because of the various dependencies on other packages. **[InstallRecipes](InstallRecipes.md)** collects a number of examples of how MDAnalysis has been successfully installed on various systems; possibly one of the recipes applies to your situation, too, and you can simply copy and paste.


### Getting the source ###
You can either get the source tar files or check out the source code from the [git](git.md) repository.

#### Source tar files ####
In order to download the source packages, you need to go to the PyPI repositories:
  * https://pypi.python.org/pypi/MDAnalysis (main package)
  * https://pypi.python.org/pypi/MDAnalysisTests (for the [tests](UnitTests.md))
and download the tar files from there.

#### Git repository ####
Alternatively, you can check out the MDAnalysis directory from the [git](git.md)
repository at http://mdanalysis.googlecode.com. See the [source checkout instructions](http://code.google.com/p/mdanalysis/source/checkout) for details. In most cases
simply do
```
git clone https://code.google.com/p/mdanalysis
cd mdanalysis
```

If you have cloned the repository before, all you need to do is
```
git pull
```
to update your files.

### Standard installation from source ###
Currently MDAnalysis is only distributed in source form. You will need
a [C compiler](http://en.wikipedia.org/wiki/List_of_compilers#C_compilers).

We recommend (for python >= 2.6) the following way of installing in your home directory (a so called "user" installation) ::
```
python setup.py build
python setup.py install --user
```

If you want to install in the system wide python directory you will probably require administrative privileges, and so do
```
sudo python setup.py install
```

It is also possible to use `--prefix`, `--home`, or `--user` options for
`setup.py` to install in a different (probably your private) python
directory hierarchy.

If you have problem at this stage then have a look at the operating
system specific notes at the end of this file or look in the issue
tracker --- maybe the problem is recognized and a workaround can be
found in the comments

Unfortunately, installing python packages is not always completely straightforward, so please read through these docs and perhaps [ask on the mailing list](http://groups.google.com/group/mdnalysis-discussion).


#### Selecting an installation directory ####

In you should be able to use
```
 python setup.py install --prefix LOCAL_DIRECTOY
```
or any of the other options of [distutil's setup.py to install in alternative directories](http://docs.python.org/install/index.html#alternate-installation).

Please ask on the [mailing list](http://groups.google.com/group/mdnalysis-discussion) for help with installation and/or [file a bug through the issue tracker](http://code.google.com/p/mdanalysis/issues/list).


### Easy Install ###

One can also use [EasyInstall](http://peak.telecommunity.com/DevCenter/EasyInstall). You should be able to do
```
easy_install [options] ./mdanalysis
```
for a standard installation (see easy\_install docs for details).

If you downloaded a tar archive from the web you can
  1. unpack the archive
  1. easy\_install the unpacked archive
It would look similar to
```
tar -zxvf MDAnalysis-0.7.3.tar.gz
easy_install MDAnalysis-0.7.3 MDAnalysis[tests,analysis]
```
Note that the additional requirements `[tests,analysis]` tell easy\_install to download and install additional packages required for this functionality.



### Developer installation ###

A _developer installation_ (that immediately reflects changes to the
sources) can be done with
```
cd ./mdanalysis
python setup develop [options]
```

For testing one can simply use
```
python setup.py develop --install-dir=$HOME/python-lib/ --script-dir=$HOME/bin
```
This builds and installs a working version in
`~/python-lib/MDAnalysis` which is linked to the unpacked source. Then add `$HOME/python-lib` to the `PYTHONPATH`
```
export PYTHONPATH=${PYTHONPATH}:$HOME/python-lib
```
However, the developer installation above is probably cleaner.

# Additional non-standard packages #

See the operating system specific notes below for hints how to get the necessary packages through the native package management system. Please add your own (eg for RPM based systems which the developers are not using heavily).

  * _python-dev_ includes `Python.h`, which is required for compiling.
  * _numpy_ is used at the compilation stage to find maths libraries.
  * _scipy_, _biopython_, _netcdf4-python_ are only needed when one wants to use all MDAnalysis functions but are not required for compiling.


## Required python packages ##
In order to make _full use_ of the library in your own python code you will need at least
  * [numpy](http://numpy.scipy.org/) of version 1.0.3 or greater
  * [scipy](http://www.scipy.org/)
  * [matplotlib](http://matplotlib.sourceforge.net/)
  * [BioPython](http://biopython.org/)'s Bio.PDB (MDAnalysis will complain if you don't have Biopython installed (even though it is not used by default anymore))
  * [networkx](http://networkx.lanl.gov/) --- for analysis of lipid leaflets via MDAnalysis.analysis.leaflet
  * [GridDataFormats](http://pypi.python.org/pypi/GridDataFormats)
  * [netcdf4-python](http://code.google.com/p/netcdf4-python/): If you want to operate on AMBER binary trajectories (NetCDF) then you will need to have working [NetCDF](http://code.google.com/p/mdanalysis/wiki/netcdf) support based on the netcdf4-python package.
    * If you get a _ValueError: did not find HDF5 headers_ during      installation then you should install the _hdf5 development package_ through your package manager (see below for some hints on how to do this for [Linux](#Linux.md) and [Mac OS X](#Mac_OS_X.md)).


## Optional python packages ##

In order to run the [UnitTests](UnitTests.md) you will need
  * numpy >=1.3
  * nose >= 0.10

# OS specific notes #

See also InstallRecipes for "copy & paste" installation instructions for various operating systems and versions.

## Linux ##
  * Install prerequisite packages on **[Ubuntu](http://www.ubuntu.com/)** or **[Debian](http://www.debian.org/)** with
```
 sudo apt-get install python-dev python-cython python-numpy g++
```
> Install packages needed for full functionality
```
 sudo apt-get install python-scipy python-matplotlib python-biopython  libhdf5-dev 
```
> (If you have issues with the netcdf libraries see the [wiki page on netcdf](http://code.google.com/p/mdanalysis/wiki/netcdf)); the netcdf and libhdf5-dev packages are  required if you want to be able to process AMBER netcdf (binary) trajectories.)
  * Tested with GNU compilers



## Mac OS X ##
### Mac Ports ###
Tested 10.6.8+MacPorts
```
port install  py26-numpy  py26-cython
port install  py26-scipy  py26-matplotlib py26-biopython hdf5 netcdf+dap+netcdf4
```
(The netcdf and libhdf5-dev packages are only required if you want to be able to process AMBER netcdf (binary) trajectories.)

Other packages such as networkx and GridDataFormats are automagically installed when running {{python setup.py install}} (or `easy_install` or `pip`).

### fink ###
Tested with Mac OS X 10.4.11+fink (has not been tested since 2011 — feedback is welcome)

Install prerequisite packages using [fink](http://www.finkproject.org/)
```
     fink install cython-py25 scipy-core-py25 scipy-py25
```
Install packages needed for full functionality
```
     fink install  matplotlib-py25 biopython-py25
```

**NOTE**: Use the Apple-provided `gcc`, not the fink provided one. Apparently, only Apple's gcc has the `-arch` flag that appears to be required for `python setup.py install`. (Thanks to Justin Lemkul for pointing it out; see [Issues after Installation](http://groups.google.com/group/mdnalysis-discussion/browse_thread/thread/1bf033ed1d3bb915)). In order to install Apple's gcc you will need to install the Apple Developer Tools that come on one of the additional installation disks with the Mac or install [Xcode](http://developer.apple.com/xcode/) from the web (note that Xcode 3.x is free and can be obtained by registering online for the Apple Developer Connection).



# Older versions #

The above instructions should work for the latest version. Older versions of MDAnalysis had different requirements.


## up to 0.7.2 ##

A LAPACK library is required (standard [LAPACK/BLAS](http://www.netlib.org/lapack), [ATLAS](http://www.netlib.org/atlas/), or the [Intel Math Kernel Library](http://software.intel.com/en-us/intel-mkl/) (MKL); on Mac OS X we simply use the native fast [VecLib framework](http://developer.apple.com/hardwaredrivers/ve/vector_libraries.html) so you don't have to install anything else).

### Fast math libraries ###

On Mac OS X we use the systems vecLib for fast linear algebra and
nothing needs to be configured. On Linux you will probably want to use
something such as ATLAS or the Intel Math Kernel Libraries for better
performance for the rms fitting.

The fast math library paths are set in `setup.cfg`. By default we ship a template file `setup.cfg.template` that you can
```
cp setup.cfg.template setup.cfg
```
and edit. See the file itself for examples and the ones below.

#### Example MKL ####

Linux section of `setup.cfg` (your paths are probably different from the
ones in this example)::

```
  [linux]
  fast_numeric_include = /opt/intel/cmkl/10.0.5.025/include
  fast_numeric_linkpath = /opt/intel/cmkl/10.0.5.025/lib/em64t
  fast_numeric_libs = mkl_lapack mkl guide
```


#### Example ATLAS LAPACK ####

If you want to use the ATLAS LAPACK libraries use something such as ::

```
[linux]
fast_numeric_include = /usr/include
fast_numeric_linkpath = /usr/lib/atlas
fast_numeric_libs = lapack
```

Your paths could be different. Use a command such as
```
locate lapack
```
or your package manager to get an idea of where the files are to be found.

On some versions of Ubuntu we had to use
```
[linux]
# Ubuntu 9.04 i686
fast_numeric_include = /usr/include
fast_numeric_linkpath = /usr/lib/sse2/atlas
fast_numeric_libs = lapack
```

### Adding lapack packages ###
```
 sudo apt-get install liblapack-dev   # see below for notes on fast_numeric_*
```