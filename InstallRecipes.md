[Installing MDAnalysis](Install.md) can be a bit daunting at first because it requires a fairly complete python environment that is set up for scientific calculations.

Even though the [Install](Install.md) page and the [INSTALL](http://code.google.com/p/mdanalysis/source/browse/INSTALL) file in the distribution explain the basic things that are needed to get MDAnalysis up and running, we found that sometimes users still struggle to make everything work.

On this page we want to provide example installation scenarios that worked for people. This should provide new users with hints at what might work for them. With a bit of luck it becomes as simple as cut and paste but in any case it should provide inspiration for what to look out for.

_Please contribute your own successful installation protocols to this page by editing it directly or adding them as comments._ Others will thank you. Alternatively, email them to one of the maintainers or add them to the comments on this page. Thank you!

# Changes with releases #
The recipes below typically mention the release of MDAnalysis. Installation can change between releases thus you should always check the release and the release notes (either by looking at the [CHANGELOG](http://code.google.com/p/mdanalysis/source/browse/CHANGELOG) in the distribution or the release notes here on the wiki, accessible through the contents panel on the left hand side).

Also note the following:
  1. From **release >0.7.4 onwards**, the source code repository was [changed from subversion to git](svn2gitConversion.md) and hence source code checkouts need to be done with git (as described in the [source code checkout instructions](http://code.google.com/p/mdanalysis/source/checkout)).
  1. MDAnalysis **up to release 0.7.2** required the "fast math libraries" (lapack); they are not needed for >= 0.7.3 and any mentioning of them can be ignored when applying the recipes below to >= 0.7.3.
  1. MDAnalysis **from release 0.7.2 onwards** does _not_ require the user to have _cython_ anymore.
  1. MDAnalysis **up to release 0.7.0** required the [pyrex](http://www.cosc.canterbury.ac.nz/greg.ewing/python/Pyrex/) package. From [0.7.1](ReleaseNotes071.md) onwards, pyrex was replaced by [cython](http://cython.org). The recipes below always mention _cython_ so keep in mind that you might have to install _pyrex_ for older releases.

# Table of Recipes #


# Linux #
## Debian 7.6 "Wheezy" ##
  * [MDAnalysis 0.8.1](ReleaseNotes081.md) (release)
  * [Debian](https://www.debian.org/) 7.6  (Linux\_x86\_64)
  * Python 2.7.3
  * installation in a user's `$HOME/.local/lib directory.`

Installation of everything needed to build and fully use [MDAnalysis 0.8.1](ReleaseNotes081.md):
```
sudo aptitude install build-essential python-dev python-setuptools python-pip
sudo aptitude install python-numpy python-scipy python-matplotlib  python-biopython python-networkx  
sudo aptitude install libhdf5-serial-dev libnetcdf-dev
sudo pip install netCDF4   # no Debian package available
```
(The [netcdf](netcdf.md) Python wrapper is installed into /usr/local but if you prefer you can also do a user installation instead, `pip install --user netCDF4`, which will install it under `$HOME/.local`).

Install MDAnalysis in my home directory (under `$HOME/.local`):
```
pip install --user MDAnalysis MDAnalysisTests
```

Additional goodies:
```
sudo aptitude install ipython ipython-notebook 
sudo aptitude install python-virtualenv virtualenvwrapper 
```
For developers:
```
sudo aptitude install cython python-sphinx 
```

## Ubuntu 14.04 "Trusty Tahr" ##
  * [MDAnalysis 0.8.1](ReleaseNotes081.md) (release)
  * [Ubuntu](http://www.ubuntu.com/) 14.04 LTS (Linux\_x86\_64)
  * Python 2.7.6
  * installation in a user's `$HOME/.local/lib directory.`

Installation of everything needed to build and fully use [MDAnalysis 0.8.1](ReleaseNotes081.md):
```
sudo aptitude install build-essential python-dev python-setuptools python-pip
sudo aptitude install python-numpy python-scipy python-matplotlib  python-biopython python-networkx  
sudo aptitude install libhdf5-serial-dev libnetcdf-dev
sudo pip install netCDF4   # no Ubuntu package available
```
Install MDAnalysis in my home directory:
```
pip install --user MDAnalysis MDAnalysisTests
```

Additional goodies:
```
sudo aptitude install ipython ipython-notebook 
sudo aptitude install python-virtualenv virtualenvwrapper 
```
For developers:
```
sudo aptitude install cython python-sphinx 
```


## Ubuntu 12.04 "Precise Pangolin" ##
  * [MDAnalysis 0.8.1](ReleaseNotes081.md) (release)
  * [Ubuntu](http://www.ubuntu.com/) 12.04 LTS Server (Linux\_x86\_64)
  * Python 2.7.3
  * installation in a user's `$HOME/.local/lib directory.`

Installation of everything needed to build and fully use [MDAnalysis 0.8.1](ReleaseNotes081.md):
```
sudo aptitude install build-essential python-dev python-setuptools python-pip
sudo aptitude install python-numpy python-scipy python-matplotlib  python-biopython python-networkx  
sudo aptitude install libhdf5-serial-dev libnetcdf-dev
sudo pip install netCDF4   # no Ubuntu package available
```
Install MDAnalysis in my home directory:
```
pip install --user MDAnalysis MDAnalysisTests
```

Additional goodies:
```
sudo aptitude install ipython
sudo aptitude install python-virtualenv virtualenvwrapper 
```
For developers:
```
sudo aptitude install cython python-sphinx 
```

## Ubuntu 11.10 "Oneiric Ocelot" - installed as user ##
  * MDAnalysis svn trunk at [r914](https://code.google.com/p/mdanalysis/source/detail?r=914) when tested
  * [Ubuntu](http://www.ubuntu.com/) 11.10 Desktop (Linux\_x86\_64)
  * Python 2.7.2+ (default, Oct  4 2011, 20:06:09) [4.6.1](GCC.md) on linux2
  * installation in a user's `$HOME/.local/lib directory.`

Here is a Python script that will perform an installation of the latest build of the mdanalysis package (along with its dependencies) on a fresh install of Ubuntu 11.10:

```
#!/usr/bin/env python
# Author: Tyler Reddy
'''Nov. 12/ 2011: Install latest build of mdanalysis for a user (NOT system-wide) on fresh Ubuntu 11.10 system.

You will need administrative privileges and should call the script in an empty folder with:

python mdanalysis_Ubuntu_11_10_install.py

You may occasionally have to enter your sudo password and press "Y" to accept
package installations.'''

import subprocess, os

#install some preliminary packages using apt-get:
print 'Install some preliminary packages using apt-get:\n'
subprocess.call(['sudo','apt-get','install','python-setuptools','cython','python-nose','build-essential'])

#install scientific packages:
print 'Install scientific packages: \n'
subprocess.call(['sudo','apt-get','install','python-numpy','python-scipy','python-biopython','python-dev'])

#install subversion package, which is used for checking out the latest source:
print 'install subversion package: \n'
subprocess.call(['sudo','apt-get','install','subversion'])

#make an appropriately named folder for the purpose of the svn checkout of source:
os.mkdir('mdanalysis')

#this code will assume that the user is not an MDA developer and will use subversion to check out a read-only source code anonymously over HTTP:
print 'check out the mdanalysis source code anonymously over HTTP:\n'
subprocess.call(['svn','checkout','http://mdanalysis.googlecode.com/svn/trunk/', 'mdanalysis'])

#now enter the mdanalysis directory (which must be in the current working directory based on the above subversion call):
os.chdir('mdanalysis')

#perform a 'user' (NOT system-wide) installation:
subprocess.call(['python','setup.py','build'])
subprocess.call(['python','setup.py','install','--user'])
```

If you copy the above code into a file named
```
mdanalysis_Ubuntu_11_10_install.py
```
(preferably in an empty folder) you can run the program with:
```
python mdanalysis_Ubuntu_11_10_install.py 
```

You will need administrative privileges for the
```
sudo apt-get
```
commands that are issued by the script to obtain packages (if you do not have them already), but the installation itself will not be system wide and will install to a user's
```
$HOME/.local/lib directory
```

This script is currently intended to install the latest build of mdanalysis on Ubuntu 11.10, but could certainly be modified to deal with other operating systems or for custom setup scenarios; it is intended to get mdanalysis up and running quickly and obviously not targeted at mdanalysis developers who will want to write back to the repository, although it could also be modified for this type of setup as well




## Ubuntu 11.04 "Natty Narwhal" - installed as user ##
  * MDAnalysis svn trunk at [r878](https://code.google.com/p/mdanalysis/source/detail?r=878)
  * [Ubuntu](http://www.ubuntu.com/) 11.04 Desktop (Linux\_x86\_64)
  * python 2.7.1+ _Python 2.7.1+ (r`271:86832, Apr 11 2011, 18:13:53) [4.5.2](GCC.md) on linux2_
  * installation in a user's $HOME/.local/lib directory.

Install easy\_install (i.e. python setuptools) if it is not available and other packages for compilation and testing
```
  sudo aptitude install python-setuptools  python-cython python-nose build-essential
```
The `build-essential` package just gets everything needed to compile code such as gcc C-compiler.


Install all the scientific packages (note changes: python-dev and cython)
```
   sudo aptitude install python-numpy scipy python-biopython python-dev
```
([NumPy](http://numpy.scipy.org/) is an absolutely essential component.

Get the sources from http://code.google.com/p/mdanalysis (either [via subversion checkout](http://code.google.com/p/mdanalysis/wiki/Install) or a tarball). I am using the development trunk from subversion (svn):
```
 svn checkout http://mdanalysis.googlecode.com/svn/trunk/ mdanalysis
 cd mdanalysis
```

Build and Install in directory ~/local/lib/python2.7/site-packages/MDAnalysis-0.7.5\_devel-py2.7-linux-x86\_64.egg/MDAnalysis/

```
python setup.py build
python setup.py install --user
```

## Ubuntu 10.04 "Lucid Lynx", upgrade to latest release from internet ##
  * [MDAnalysis 0.7.4](http://pypi.python.org/pypi/MDAnalysis/0.7.4) from [PyPI](http://pypi.python.org/) (automagic upgrade installation from internet)
  * Ubuntu 10.04 "Lucid Lynx"
  * Python 2.6.5 (r``265:79063, Apr 16 2010, 13:57:41)
  * installation in my own user directories

The machine has C compiler and NumPy installed, together with [easy\_install](http://packages.python.org/distribute/easy_install.html). Then upgrading (option `-U`) my current installation is as simple as
```
easy_install-2.6 -U --install-dir /sansom/gfio/oliver/Library/x86_64-unknown-linux-gnu/lib/python2.6  MDAnalysis
```
`easy_install -U` automatically searches the Python Package Index [PyPI](http://pypi.python.org/), finds the download URL for the most recent MDAnalysis releases (0.7.4 in this case), downloads, and installs.

## Ubuntu 10.04 "Lucid Lynx", basic ATLAS libraries ##
  * [MDAnalysis-0.6.3-rc1.tar.gz](http://code.google.com/p/mdanalysis/downloads/detail?name=MDAnalysis-0.6.3-rc1.tar.gz)
  * Ubuntu Server 10.04 "Lucid Lynx"
  * Python _2.6.5 (r``265:79063, Apr 16 2010, 13:57:41) [4.4.3](GCC.md) on linux2_
  * _libatlas3gf-base_ package
  * all python packages are installed in `~/.local/lib/python2.6/site-packages`

I decided that all my python modules should go into my home directory instead of being installed in the system-wide directories. To this end I set (for once and all) the defaults in `~/.pydistutils.cfg`. In fact, I am installing packages in a special directory `~/.local/lib/python2.6/site-packages` which is automagically found by python 2.6.

To do the same, create/open the file
```
nano ~/.pydistutils.cfg
```
with your favourite editor ([nano](http://www.nano-editor.org/docs.php) is nice and simple) and enter the following
```
# User installation:
# http://peak.telecommunity.com/DevCenter/EasyInstall#mac-os-x-user-installation
# http://peak.telecommunity.com/DevCenter/EasyInstall#downloading-and-installing-a-package
# note python 2.6 uses ~/.local automatically in the PYTHONPATH
# http://docs.python.org/whatsnew/2.6.html

[install]
install_lib = ~/.local/lib/python$py_version_short/site-packages
install_scripts = ~/bin
```

Install prerequisites:
```
sudo aptitude install  python-setuptools  python-cython python-nose build-essential
sudo aptitude install  python-numpy python-scipy python-biopython  libatlas-headers libatlas3gf-base
```
Get source code (either download from web site or via commandline like here):
```
wget http://mdanalysis.googlecode.com/files/MDAnalysis-0.6.3-rc1.tar.gz
```

Optional, test if the file was downloaded uncorrupted:
```
sha1sum MDAnalysis-0.6.3-rc1.tar.gz  # check that the file is in order
```
Compare the output of the [sha1sum](http://www.gnu.org/software/coreutils/manual/html_node/sha1sum-invocation.html) command
```
82515095c4e2d42f508596228e6acd364248b45d  MDAnalysis-0.6.3-rc1.tar.gz
```
to the SHA1 checksum on the [MDAnalysis-0.6.3-rc1.tar.gz download page](http://code.google.com/p/mdanalysis/downloads/detail?name=MDAnalysis-0.6.3-rc1.tar.gz). If they are not identical, remove the downloaded file and try again. If this does not work, ask for help on the [mdnalysis-discussion mailing list](http://groups.google.com/group/mdnalysis-discussion).

Unpack
```
tar -zxvf MDAnalysis-0.6.3-rc1.tar.gz
cd MDAnalysis-0.6.3-rc1
```

Prepare for compiling against the standard ATLAS libraries.
```
cp setup.cfg.template setup.cfg
nano setup.cfg # edit setup.cfg with your favourite editor
```
Enable the following lines in `setup.cfg`:
```
fast_numeric_include = /usr/include
fast_numeric_linkpath = /usr/lib/atlas
fast_numeric_libs = lapack
```

Compile MDAnalysis and install
```
python setup.py build
python setup.py install
```

Optional, test the package (see [UnitTests](UnitTests.md))
```
cd ~   # do NOT test in the unpackaged source, will fail because libs are not found
python # inside python (do not type the prompt >>>)
>>> import MDAnalysis.tests
>>> MDAnalysis.tests.test(label='full', extra_argv=['--exe'])
```
Should run cleanly without any fails (F) or errors (E); submit a bug if otherwise.

Notes:
  * The 0.6.3-rc1 actually has a number of E's but this has been fixed in [r356](https://code.google.com/p/mdanalysis/source/detail?r=356).
  * One could also use the SSE2 optimized ATLAS libs; see the other Linux example.
  * If you do not customize `~/.pydistutils.cfg` then you can use the above recipe in ordr to install into the default site-wide directories. At the last step you will probably have to run
```
  sudo python setup.py install
```


## Ubuntu 9.04 "Jaunty Jackalope", ATLAS SSE2 (libatlas3gf-sse2), installed in custom directory ##
  * MDAnalysis svn trunk at [r341](https://code.google.com/p/mdanalysis/source/detail?r=341)
  * [Ubuntu](http://www.ubuntu.com/) 9.04
  * python 2.6.2: _Python 2.6.2 (release26-maint, Apr 19 2009, 01:56:41) [4.3.3](GCC.md) on linux2_
  * installation in a private directory (not the site-wide one)
  * use [ATLAS](http://math-atlas.sourceforge.net/) for fast linear algebra (SSE2 version on i686), _libatlas3gf-sse2_ package

Install easy\_install (i.e. python setuptools) if it is not available and other packages for compilation and testing
```
  sudo aptitude install python-setuptools  python-cython python-nose build-essential
```
The `build-essential` package just gets everything needed to compile code such as gcc C-compiler.


Install all the scientific packages, including the [ATLAS linear algebra library](http://math-atlas.sourceforge.net/)
```
   sudo aptitude install python-numpy python-scipy python-biopython  libatlas-headers libatlas3gf-sse2 
```
([NumPy](http://numpy.scipy.org/) is an absolutely essential component. Here I use sse2 optimized ATLAS but you might want to choose a different package for your architecture; find possible choices with `aptitude search atlas`.)


Get the sources from http://code.google.com/p/mdanalysis (either [via subversion checkout](http://code.google.com/p/mdanalysis/wiki/Install) or a tarball). I am using the development trunk from subversion (svn):
```
 svn checkout http://mdanalysis.googlecode.com/svn/trunk/ mdanalysis
 cd mdanalysis
```
If you have already checked out `mdanalysis` previously, simply _update the sources_ with
```
 cd mdanalysis
 svn update
```

Configure the package so that the fast ATLAS linear algebra library can be found:
```
cp setup.cfg.template setup.cfg
```
Edit `setup.cfg` and add the following lines in the ```[linux]`` section:
```
[linux]
# fast numeric linear algebra on Ubuntu 9.04 with libatlas3gf-sse2
fast_numeric_include = /usr/include
fast_numeric_linkpath = /usr/lib/sse2/atlas
fast_numeric_libs = lapack
```
(Only add the lines starting with fast\_xxx; do not repeat the `[linux]` header.)


Now we are ready to build and install:
```
 easy_install-2.6 --install-dir /sansom/gfio/oliver/Library/i686-pc-linux-gnu/lib/python2.6 .
```
Things to note:
  * The '.' for the current directory at the end of the command is _essential_. It tells easy\_install to build whatever it can find in the current directory.
  * You can change the installation directory to a different path. I am keeping installations for multiple operating systems and architectures in one network-mounted directory and hence I distinguish by architecture strings such as "i686-pc-linux-gnu".)

**This should do everything and you should have a fully working installation.** See UnitTests for how to run the tests that come with MDAnalysis.

Optional (but extremely useful) packages:
```
 sudo aptitude install ipython python-matplotlib   # optional but very useful 
```
If these are not installed, MDAnalysis will still work but we recommend them for daily use with MDAnalysis.


### Trouble shooting ###

```
/usr/bin/ld: cannot find -llapack
collect2: ld returned 1 exit status
error: Setup script exited with error: command 'gcc' failed with exit status 1
```

The linear algebra library _liblapack_ cannot be found.
  * Configure `setup.cfg` as described above.
  * Check if the `liblapack.so` file is really in the `/usr/lib/sse2/atlas` directory, using e.g.
```
 locate atlas; locate liblapack.so
```
> Also, check where the package installed its files with
```
 dpkg -L libatlas3gf-sse2
```
  * Adjust the `fast_numeric_linkpath` accordingly to the directory in which `liblapack.so` was found.


## CentOS 5: user installation ##
  * MDAnalysis 0.7.4, installation directly from the web
  * [CentOS](https://www.centos.org/) centos-release-5-6.el5.centos.1, including C++ compiler g++.
  * Python 2.6.6 ([r266](https://code.google.com/p/mdanalysis/source/detail?r=266):84292, May 21 2011, 15:06:44) (separately compiled and installed)
    * NumPy 1.6 (separately compiled and installed)
    * [setuptools](http://pypi.python.org/pypi/setuptools) with `easy_install-2.6` separately installed (setuptools-0.6c11-py2.6.egg)
    * Note that `PYTHONPATH` was adjusted as to contain the Python2.6 directories instead of the default CentOS Python2.4 directories.
  * _user installation_ (by explicitly setting the installation directory to `~/.local/lib/python2.6/site-packages`)

`easy_install` can directly install a source distribution from the web:
```
easy_install-2.6 --script-dir ~/bin --install-dir ~/.local/lib/python2.6/site-packages http://mdanalysis.googlecode.com/files/MDAnalysis-0.7.4.tar.gz
```
This installs the package in the same place where `python2.6 setup.py install --user` would put it.

Optionally, we can also install the [MDAnalysisTestData](MDAnalysisTestData.md):
```
easy_install-2.6 --script-dir ~/bin --install-dir ~/.local/lib/python2.6/site-packages http://mdanalysis.googlecode.com/files/MDAnalysisTestData-0.7.4.tar.gz
```
(but this will take a little bit longer to download at ~18 MB). However, once we have both we can run the UnitTests to check that everything is working. Just first make sure you have `nose` installed:
```
easy_install-2.6 -U --script-dir ~/bin --install-dir ~/.local/lib/python2.6/site-packages 'nose>=0.10.0'
```
Run the UnitTests suite from the command line:
```
nosetests-2.6 --verbose -w ~/.local/lib/python2.6/site-packages/MDAnalysis-0.7.4-py2.6-linux-x86_64.egg
```
You should only get "Known Failures" (warnings are typically not a problem). Here I get as final output:
```
Ran 353 tests in 371.082s

OK (KNOWNFAIL=4)
```
Report any problems to the mailing list.



# Mac OS X #

## Mountain Lion 10.8.2 with MacPorts: private user directory ##
  * MDAnalysis-0.7.6
  * Mac OS X 10.8.2
  * XCode 4.5.1 (4G1004)
  * MacPorts (tree v 2.1.2)
  * all python dependencies installed using macports
  * /opt/local/bin/python 2.6.8
  * installation in a private user directory (i.e. ~foo/.local/)

I have tested this end-to-end on several 10.8.2 machines. Be warned (a) it can take a few hours and (b) will require a few Gb on your machine. So if you have a laptop, please plug in the power and make sure you have fast internet connection.

The first step is to install Xcode - this is also described on the [MacPorts install page](http://www.macports.org/install.php). On Mountain Lion and Lion this is done through the Mac App Store which is in your dock. Search for Xcode and install (it is free) - you will be prompted for your Apple ID password. Xcode is large (~2 Gb). The download progress is shown by a little bar on the Mac App Store icon in the Dock.

Once it has downloaded, run the Xcode app in your /Applications/ folder. It will ask you to install something. Agree. When it has finished, open Preferences | Downloads and tick the "Command Line Tools" box. This will then download and install the compilers that MacPorts will need. The step in this para is I think not required for Lion. For older versions of Mac OS X you have to download Xcode from the Apple website (and may I think need to "register" as a developer to do so).

Now go to the [MacPorts](http://www.macports.org/install.php) website and download the dmg installer for your operating system. Use it to install MacPorts.

If you open a Terminal (you can find this in /Applications/Utilities) and type
```
 which port
```
it should report back
```
 /opt/local/bin/port
```
All other MacPorts will also be installed in /opt/local so it will not affect your native Mac install. This next step can take up to 2 hours, depending on your network connection and how many cores your machine has but should install all the dependencies we need for MDAnalysis. You have a choice here: all the dependencies come in different flavours depending on whether you want to use python 2.6 or 2.7. I've used 2.6 here and have not tested 2.7 but you could try it if you want to.
```
 sudo port install py26-numpy py26-scipy py26-nose py26-biopython py26-cython wget
```
This will take a surprisingly long time, mainly because it will install the gcc compiler for scipy. The following are optional but you might as well
```
 sudo port install py26-ipython py26-matplotlib 
```
This will also have the effect of installing a second python in /opt/local/bin. If you now type
```
 which python
```
it should say
```
 /opt/local/bin/python
```
You'll notice that MacPorts has modified your .bash\_profile to add `/opt/local/bin` to your $PATH so that it is ahead of `/usr/bin` and so this new python will be run in preference to the standard Mac installed one (and likewise the modules associated with it will be loaded in preference - e.g. [numpy](numpy.md) comes with Mountain Lion).

If it fails at any time, say py26-numpy fails, it is always worth doing a
```
 sudo port clean py26-numpy
 sudo port install py26-numpy
```
as this often fixes any problem. Now we are in a position to install MDAnalysis. In a terminal
```
 wget http://mdanalysis.googlecode.com/files/MDAnalysis-0.7.6.tar.gz
 tar zxvf MDAnalysis-0.7.6.tar.gz
 cd MDAnalysis-0.7.6
 python setup.py build
 python setup.py install --user
 cd ..
python
>>> import MDAnalysis
>>> MDAnalysis.__path__
['/Users/foo/.local/lib/python2.6/site-packages/MDAnalysis-0.7.6-py2.6-macosx-10.8-x86_64.egg/MDAnalysis']
```
The cd is important - if you try running python from within the folder it will complain when you try and import the module. This will put MDAnalysis and any other dependencies (like GridDataFormats) in ~user/.local/lib/python2.6/site-packages/. You could opt for a system wide installation but it is not obvious to me the best place to put this. If you tried putting it in /opt/local I think MacPorts would get confused as it hadn't put it there and I don't think putting it in /Library/ is a good idea. Please edit if you know better!

Now we should test
```
wget http://mdanalysis.googlecode.com/files/MDAnalysisTests-0.7.6.tar.gz
tar zxvf MDAnalysisTests-0.7.6.tar.gz
cd MDAnalysisTests-0.7.6
python setup.py build
python setup.py install --user
cd ..
python
>>> import MDAnalysis.tests
>>> MDAnalysis.tests.test(label="full")
```
This will take some time. If you are impatient see the [UnitTests](http://code.google.com/p/mdanalysis/wiki/UnitTests) page for ways of running on multiple cores or only running a subset of the tests.

## Lion Mac OS X 10.7.5 ##
[Issue 142](https://code.google.com/p/mdanalysis/issues/detail?id=142) describes that on a Lion installation with MacPorts the Python 2.7 installation appears broken with the following error
```
ld: library not found for -lgomp
clang: error: linker command failed with exit code 1 (use -v to see invocation)
error: Setup script exited with error: command '/usr/bin/clang' failed with exit status 1
```
The report suggests to use Python 2.6, i.e. (as above) install pre-requisites such as _py26-python_, _py26-cython_,  _py26-scipy_, etc and then use
```
easy_install-2.6 -U MDAnalysis
```
or
```
python2.6 setup.py install
```
(although you might have to modify `PYTHONPATH` for the latter to work if your default installation is Python 2.7).



## Leopard 10.5.8 with macports ##
  * MDanalysis 0.7.7
  * XCode 3.1.4
  * MacPorts 2.1.2

This recipe worked on my Mac which had the above mentioned versions of XCode and MacPorts installed:

1) After downloading the MDAnalsyis source code, use MacPorts to install the python packages that MDAnalysis needs to work. You have to use hdf5-18, it seems that hdf5 is incompatible with netcdf

```
port install py26-numpy py26-cython py26-scipy py26-matplotlib py26-ipython hdf5-18 netcdf +netcdf4 +dap configure.compiler=gcc-4.2
```

Installation of iPython is optional.

Next, when building and installing MDanalysis, use again gcc-4.2, instead of gcc-4.0, because MDanalysis does not seem to compile with gcc-4.0, apparently (one of) the reasons is that gcc-4.0 does not recognize the -fopenmp flag.



## Leopard 10.5.8 with fink: private user directory ##
  * MDAnalysis svn trunk at [r341](https://code.google.com/p/mdanalysis/source/detail?r=341)
  * Mac OS X 10.5.8
  * [fink](http://www.finkproject.org/) installed for GNU software
  * python 2.6.5: _Python 2.6.5 (r``265:79063, May 18 2010, 17:13:04) [4.0.1 (Apple Inc. build 5493)](GCC.md) on darwin_
  * installation in a private directory (not the site-wide one)

Install easy\_install (i.e. python setuptools) if it is not available and other packages for compilation and testing
```
  fink install setuptools-py26 cython-py26 nose-py26
```
(You also need a C-compiler. If you do not have the Apple developer tools installed with the Apple gcc then install a recent version of gcc with
```
  fink install gcc44
```
gcc42 or gcc40 will also do the job.)

Install all the scientific packages
```
   fink install scipy-py26 biopython-py26
```
(Note: _scipy-py26_ also installs its dependency _scipy-core-py26_ which is [NumPy](http://numpy.scipy.org/), an absolutely essential component.)


Get the sources from http://code.google.com/p/mdanalysis (either [via subversion checkout](http://code.google.com/p/mdanalysis/wiki/Install) or a tarball). I am using the development trunk from subversion (svn):
```
 svn checkout http://mdanalysis.googlecode.com/svn/trunk/ mdanalysis
 cd mdanalysis
```
If you have already checked out `mdanalysis` previously, simply _update the sources_ with
```
 cd mdanalysis
 svn update
```

Now we are ready to build and install:
```
 easy_install-2.6 --install-dir $HOME/Library/i386-apple-darwin9.8.0/lib/python2.6 .
```
(Note that you can change the installation directory to a different path. I am keeping installations for multiple operating systems and architectures in one network-mounted directory and hence I distinguish by architecture strings such as "i386-apple-darwin9.8.0".)

**This should do everything and you should have a fully working installation.** See UnitTests for how to run the tests that come with MDAnalysis.

Optional (but extremely useful) packages:
```
   fink install ipython-py26 matplotlib-py26   # optional but very useful 
```
If these are not installed, MDAnalysis will still work but we recommend them for daily use with MDAnalysis.

## Snow Leopard 10.6.4 with MacPorts: private user directory ##
  * MDAnalysis svn trunk
  * Mac OS X 10.6.4
  * [MacPorts](http://www.macports.org/) installed for GNU software
  * Python 2.6.1 (r 261:67515, Feb 11 2010, 00:51:29)
  * installation in a private directory (not the site-wide one)

Install easy\_install (i.e. python setuptools) if it is not available and other packages for compilation and testing
```
  sudo port install py26-setuptools py26-nose  py26-cython 
```
(You also need a C-compiler. If you do not have the Apple developer tools installed with the Apple gcc then install a recent version of gcc with
```
  sudo port install gcc44
```
gcc42 or gcc40 will also do the job.)

Install all the scientific packages
```
   sudo port install py26-numpy py26-scipy py26-biopython 
```
[NumPy](http://numpy.scipy.org/) is an absolutely essential component.


Get the sources from http://code.google.com/p/mdanalysis (either [via subversion checkout](http://code.google.com/p/mdanalysis/wiki/Install) or a tarball). I am using the development trunk from subversion (svn):
```
 svn checkout http://mdanalysis.googlecode.com/svn/trunk/ mdanalysis
 cd mdanalysis
```
If you have already checked out `mdanalysis` previously, simply _update the sources_ with
```
 cd mdanalysis
 svn update
```

Now we are ready to build and install:
```
 python setup.py install --user
```
(This will install everything under `~/.local/lib/python2.6/site-packages` and python 2.6 will automatically find packages there.)

**This should do everything and you should have a fully working installation.** See UnitTests for how to run the tests that come with MDAnalysis.

Optional (but extremely useful) packages:
```
   sudo port install py26-ipython py26-matplotlib   # optional but very useful 
```
If these are not installed, MDAnalysis will still work but we recommend them for daily use with MDAnalysis.

# Recipe formatting #

<font size='1'>
<b>Format</b>

When adding a new recipe, make a new level-2 heading (<code>== title ==</code> in wiki markup) for each recipe. Use a bullet-point list to describe the features of your setup so that users can check easily if the recipe applies to them. Then describe in as much detail as you think useful what you did.<br>
<br>
Useful information<br>
<ul><li>version of MDAnalysis used<br>
</li><li>version of the operating system<br>
</li><li>version of python (e.g. from <code>python -v</code>)<br>
</li><li>fast linear algebra libraries used, e.g. the name of the installed package<br>
</font>