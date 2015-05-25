Some questions and problems come up repeatedly and this page collects them in one place.



# Installation #

## What software do I need to install MDAnalysis from source? ##
  * Python ≥ 2.5 and the Python development files (typically a package called "python-dev"
  * C and C++ compiler (only gcc is fully tested)
  * NumPy ≥ 1.3

See [Installing from source](Install#Installing_from_source.md) and InstallRecipes.

## Does it work under Python3k? ##
No, not at the moment.

We are looking for developers who want to try porting. See [Issue 132](https://code.google.com/p/mdanalysis/issues/detail?id=132).

## How do I install netCDF to make the Amber reader/writer work? ##
See the page on [installing netcdf](netcdf.md).

## Why do I get a segmentation fault? ##
A segmentation fault is a problem that typically occurs when libraries are incompatible with each other or the MDAnalysis libraries have been compiled for the wrong version of Python/NumPy.

Some people solved this problem by [reinstalling Python, NumPy, SciPy, and MDAnalysis](https://groups.google.com/d/topic/mdnalysis-discussion/52Npnr2M2GE/discussion).

If you need help with this problem, you should write to the mailing list and mention

  1. which operating system (version, architecture) you have
  1. which version of Python, NumPy and the gcc compiler you have installed,
  1. which version of MDAnalysis you used,
  1. how you compiled and installed it (step by step, copy and paste the commands)

Then start fresh by unpacking the tar file and do in the shell (`$` is the shell prompt and is not to be typed)
```
$ python setup.py clean
$ python setup.py build 2>&1 build.log
$ python setup.py install 2>&1 install.log
```

Show us the output from the build and install stage (in the log files).

The next step is to find out where the segmentation fault occurs: Try to run python in the [gdb debugger](http://www.gnu.org/software/gdb/); possibly that tells us in which library the segfault occurs.

Try the following in your shell :
```
$ which python
```
That gives the full path to your python (perhaps `/usr/bin/python`, mine is `/opt/local/bin/python`).

Then start `gdb`
```
$ gdb
```

Inside `gdb` we'll try to run `python` and the import command. In the following, "(gdb)" is the debugger prompt, only type commands on the (gdb) line but without the prompt, everything else is the output I get and is shown as example. _Use whatever path you got for your python for the 'file' command below._
```
(gdb) file /opt/local/bin/python
(gdb) run -c "import MDAnalysis; print MDAnalysis.__version__"
Starting program: /opt/local/bin/python -c "import MDAnalysis; print MDAnalysis.__version__"

Program received signal SIGTRAP, Trace/breakpoint trap.
0x00007fff5fc01028 in __dyld__dyld_start ()
(gdb) continue
Continuing.
....
.... lots of warnings about Could not find object file ...
....
....
Reading symbols for shared libraries . done
Reading symbols for shared libraries . done
Reading symbols for shared libraries . done
0.7.6-devel

Program exited normally.
```

The above is a successful run without a segmentation fault. If you get a segfault, show us _the whole output from your gdb session_.

With a bit of luck the debugger stops where the segfault occurs and using the 'where' command
```
(gdb) where
```
you might be able to pinpoint the shared library. That would be a necessary first step in debugging.

If a specific command segfaults then do a second run where you replace `print MDAnalysis.__version__` with your command (multiple commands can be separated by semicolons).