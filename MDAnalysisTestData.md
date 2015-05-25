**The instructions below are outdated and do not apply anymore (since 2011-12-21)**. They are kept for historical purposes. See UnitTests for explanations.

Notes on the test data are in the report for [Issue 87](https://code.google.com/p/mdanalysis/issues/detail?id=87) and [Source](Source.md).


---


MDAnalysis contains a large number of tests of the core functionality, described in UnitTests. For many tests we are using real simulation trajectories and structures to check that everything is working as expected. These tests make it very unlikely that new code or fixes introduce new bugs. The test trajectories are also used in examples throughout the documentation, allowing anyone to try out MDAnalysis right away.

The test trajectories are fairly big (about 20 GB) so we put them in a package separate from MDAnalysis. If you want to run the UnitTests or try out the examples then you must install **MDAnalysisTestData** (as described in [INSTALL](http://code.google.com/p/mdanalysis/source/browse/branches/MDAnalysisTestData/INSTALL).

# Installation #
You can download a file such as [MDAnalysisTestData-0.7.4.tar.gz](http://code.google.com/p/mdanalysis/downloads/detail?name=MDAnalysisTestData-0.7.4.tar.gz&can=2&q=#makechanges) from the [Download](http://code.google.com/p/mdanalysis/downloads/list) page, unpack it and install as usual:
```
tar -zxvf MDAnalysisTestData-0.7.4.tar.gz
cd MDAnalysisTestData-0.7.4
python setup.py install
```


You might also be able to directly install from the web if you have [easy\_install](http://packages.python.org/distribute/easy_install.html) available
```
easy_install http://mdanalysis.googlecode.com/files/MDAnalysisTestData-0.7.4.tar.gz
```

# Running tests #
See UnitTests.

# Running examples #
Once both **MDAnalysis** _and_ **MDAnalysisTestData** are installed you can access the bundles files within the `MDAnalysis.tests.datafiles` module. For instance, on my machine
```
>>> from MDAnalysis.tests.datafiles import PSF,DCD
>>> print PSF
/Users/oliver/.local/lib/python2.6/site-packages/MDAnalysisTestData-0.7.4-py2.6.egg/MDAnalysisTestData/data/adk.psf
>>>  print DCD
/Users/oliver/.local/lib/python2.6/site-packages/MDAnalysisTestData-0.7.4-py2.6.egg/MDAnalysisTestData/data/adk_dims.dcd
>>> import MDAnalysis
>>> u = MDAnalysis.Universe(PSF,DCD)
>>> print u
<Universe with 3341 atoms>
```