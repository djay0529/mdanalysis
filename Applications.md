

# Applications #

MDAnalysis is a library that facilitates the construction of software tools that can perform novel analysis tasks. Some tools are shipped with MDAnalysis in the [MDAnalysis.analysis](http://pythonhosted.org/MDAnalysis/documentation_pages/analysis_modules.html) module and [example scripts](Examples.md) in the [examples directory of the source distribution](http://code.google.com/p/mdanalysis/source/browse/package#package%2Fexamples). There are also self-contained Python packages that depend on MDAnalysis but are installed separately (effectively since MDAnalysis 0.8). We refer to them as **applications**.

## What is a MDAnalysis application? ##
A MDAnalysis application is a self-contained Python package that makes use of MDAnalysis for some of its core functionality but is distributed independently from the MDAnalysis package itself.

A typical application consists of
  * python scripts that run the analysis task
  * support library
  * `setup.py` and related files to facilitate installation with e.g. `python setup.py install`
  * documentation
  * examples
  * test cases

## How to download and install an application? ##
There are typically multiple ways to obtain and install from source although not all of them might be applicable for all applications:
  1. Released versions of the application will have a source tarball on the [Downloads](http://code.google.com/p/mdanalysis/downloads/list) page. Download the tar file, unpack, `cd` into the unpacked directory, and run `python setup.py install` (or similar).
  1. If the application is hosted on the [Python package index](https://pypi.python.org/pypi) then you might be able to simply say `pip install APPLICATION-NAME` or `easy_install APPLICATION-NAME`.
  1. You should always be able to obtain the source via git by checking out the master branch. Applications live in the top-level `applications` directory.

If you haven't installed MDAnalysis itself yet then the application will try to do this automatically. If this fails then you should first try to [install MDAnalysis yourself](Install.md) and then install the application. Feel free to ask for help on the [user mailing list](http://groups.google.com/group/mdnalysis-discussion).

## Using an application ##
Each application should come with documentation that explains how to use it after it has been installed. Typically, you will have to run a Python script that was installed into your system.

# List of applications #
  * [RotamerConvolveMD](RotamerConvolveMD.md): analysis of spin-spin label distances

# Developer information #

The **applications/** top level directory of the MDAnalysis repository contains self-contained Python packages that make use of MDAnalysis as their primary dependency. Each package should be installable on its own but make use of dependency mechanisms to automatically install MDAnalysis (and other required packages).

The idea is that an interested user can easily install a particular application without having to worry too much about MDAnalysis itself.

As a developer, you should pick a good name and store your complete package in a directory of that name. Typically, you will be only one working on this directory but because of the open source nature of the whole MDAnalysis project, other users might also start contributing. It is up to you to communicate with users on how to coordinate development.

Each application should contain

  * code in a Python module or package
  * `setup.py`
  * installable scripts (this is how a user would typically make use of the application)
  * documentation
    * usage information and an example
    * data to execute the example
    * citation information (if applicable)

If you want your application hosted with MDAnalysis then get in touch
on the [developer mailing list](https://groups.google.com/forum/?fromgroups#!forum/mdnalysis-devel).

Source code tarballs and possibly egg archives can be hosted on the download page. Adding the application to the [Python Package Index (PyPi)](https://pypi.python.org/pypi) is encouraged.