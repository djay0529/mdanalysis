The unit tests and the test data are bundled together in the package **MDAnalyisTests-_release_**. In order to run the tests, this package must be installed in addition to MDAnalysis.

Either install [MDAnalysisTests](MDAnalysisTests.md) via
```
pip install --upgrade MDAnalysisTests
```
or download the tar file, unpack, and run `python setup.py install`
or use the tests from the [git source repository](Source.md), which are located in the [testsuite/MDAnalysisTests](http://code.google.com/p/mdanalysis/source/browse/#git%2Ftestsuite) directory.

The tests require at least numpy 1.3.

You can run all tests from the commandline
```
python -c 'from MDAnalysis.tests import test; test(label="full", verbose=3, extra_argv=["--exe"])'
```
or with the `nosetests` script (just make sure you are running the right version)
```
nosetests MDAnalysisTests
```
or from within the Python interpreter: start `python` or `ipython` and type (the `>>>` is the prompt and should not be typed!)
```
>>> import MDAnalysis.tests
>>> MDAnalysis.tests.test(label="full", extra_argv=['--exe'])
```
(The `extra_argv=['--exe']` is to ensure that the test also run on Linux, see below for [details](#Details.md).) The tests take a few minutes. Check that you only get _ok_ (shown as a dot, ".") or _known failures_ (letter "K"). "DeprecationWarning" and a "RuntimeWarning" are not a problem.  _Failures_ (letter "F") or _Errors_ (letter "E") are bad. If you cannot figure out for yourself where the problems come from, ask a question on the [discussion group](https://groups.google.com/forum/#!forum/mdnalysis-discussion), including your error output and notes on which version of MDAnalysis and operating system you're using.

Fore more details see below.

**Contents**



# Quick start #
Unit tests are stored in `MDAnalysis/tests/test_*.py`. Run all of them with
```
   import MDAnalysis.tests
   MDAnalysis.tests.test(label="full")
```
and check that you only get ok (or known failures).

## Serial testing ##
For example, a successful test might look like the following
```
>>> MDAnalysis.tests.test(label="full")
Running unit tests for MDAnalysis.tests
NumPy version 1.4.0
NumPy is installed in /opt/local/Library/Frameworks/Python.framework/Versions/2.6/lib/python2.6/site-packages/numpy
Python version 2.6.5 (r265:79063, May  1 2010, 20:59:11) [GCC 4.2.1 (Apple Inc. build 5646) (dot 1)]
nose version 0.11.1
...........................KK....KK...............
----------------------------------------------------------------------
Ran 50 tests in 54.045s

OK (KNOWNFAIL=4)
<nose.result.TextTestResult run=50 errors=0 failures=0>
```

## Parallel testing ##
Running tests in parallel is **much** faster, especially on an 8-core machine:
```
import MDAnalysis.tests
MDAnalysis.tests.test(label="full", extra_argv=["--processes=8"])
```
will throw a number of (known) errors
```
Running unit tests for numpy
NumPy version 1.3.0
NumPy is installed in /sw/lib/python2.6/site-packages/numpy
Python version 2.6.5 (r265:79063, May 18 2010, 17:13:04) [GCC 4.0.1 (Apple Inc. build 5493)]
nose version 0.11.3
...................EE.EEE............E..E.....................................................................E............E.............................................

<snip>detailed output for known failures</snip>

----------------------------------------------------------------------
Ran 169 tests in 20.425s
```
Simply look at the detailed output at the end and ignore anything that contains lines such as
```
KnownFailureTest:  blabla
```
It seems that paralle nosetests do not properly honour the "knownfailure" cases.

# Coverage #
We test code coverage of the unit tests with the  [coverage](http://nedbatchelder.com/code/modules/rees-coverage.html) plugin of nose:
```
cd testsuite
rm -f .coverage .noseids testing.log
nosetests-2.7 -v --with-id \
   --with-coverage --cover-erase --cover-html-dir=htmlcov --cover-html --cover-package=MDAnalysis \
   MDAnalysisTests/test_*.py  \
   2>&1 | tee testing.log
```
Currently, this is done manually and the online **[coverage report](http://wiki.mdanalysis.googlecode.com/git/pages/quality_control/cover/index.html)** is also update manually.

In the future, this is supposed to be done as part of a continuous integration process ([Issue 134](https://code.google.com/p/mdanalysis/issues/detail?id=134)).

# Details #
We are using the NumPy testing frame work (v >= 1.3); thus, numpy **must** be installed for the tests to run at all.

## Running tests from within python ##
Run all the tests with
```
   import MDAnalysis.tests
   MDAnalysis.tests.test(label='full')
```
Some tests can take a few seconds; in order to skip the slow tests run
```
   MDAnalysis.tests.test(label='fast')
```
Additional information is displayed at a higher verbosity level (the default is
0):
```
   MDAnalysis.tests.test(label='fast', verbose=1)
```

Note that if no tests are being run then one might have to run the
tests with the `--exe` flag
```
   MDAnalysis.tests.test(label='fast', extra_argv=['--exe'])
```
(This happens when python files are installed with the executable bit set. By default the [nose testing framework](http://somethingaboutorange.com/mrl/projects/nose) refuses to use those files and must be encouraged to do so with the `--exe` switch.)

See [nose commandline options](http://somethingaboutorange.com/mrl/projects/nose/0.11.3/usage.html#extended-usage) for additional options that can be used; for instance, code coverage can also be checked:
```
  MDAnalysis.tests.test(label='full', extra_argv=['--exe', '--with-coverage'])
```

## Running tests from the command line ##

Instead of running tests from within python, one can also run them via the [nosetests](http://somethingaboutorange.com/mrl/projects/nose/0.11.2/usage.html) script that is being installed as part of the `nose ` package.

Go into the tests directory
```
cd MDAnalysis/tests
```
and invoke [nosetests](http://somethingaboutorange.com/mrl/projects/nose/0.11.2/usage.html) directly to run **all tests** on two processors in parallel ("`%`" is the shell prompt and should _not_ be typed):
```
% nosetests-2.6 --processes=2
```
(When the `-v` flag is added, more verbose output is produced.)

When you have written a **new unit test** it is helpful to check that it passes without running the entire suite. For example, in order to test everything in, say, [test\_selections.py](http://code.google.com/p/mdanalysis/source/browse/MDAnalysis/tests/test_selections.py) run
```
% nosetests-2.6 test_selections   
..............
----------------------------------------------------------------------
Ran 14 tests in 3.421s

OK
```
One can also test individual test classes. For instance, after working on the XYZReader one can check just the TestCompressedXYZReader tests with
```
% nosetests-2.6 test_coordinates:TestCompressedXYZReader
....
----------------------------------------------------------------------
Ran 4 tests in 0.486s

OK
```
where we are testing the class TestCompressedXYZReader which can be found in the module (file) [test\_coordinates.py](http://code.google.com/p/mdanalysis/source/browse/MDAnalysis/tests/test_coordinates.py#66)

If you just installed the `MDAnalysisTests` package you can also simply run
```
nosetests -v MDAnalysisTests
```

### Running tests with setuptools ###

Setuptools can also use [nose](http://somethingaboutorange.com/mrl/projects/nose) directly (and it takes care of having all the libraries in place):
```
python setup.py nosetests
```

If you have the [coverage](http://nedbatchelder.com/code/modules/rees-coverage.html) package installed, you can also check code coverage of the tests:
```
python setup.py nosetests --with-coverage --cover-package=MDAnalysis --cover-erase --cover-tests
```



## Data ##

The simulation data used in some tests are from Beckstein et al. (2009) (`adk.psf`,
`adk_dims.dcd`) or unpublished simulations (O. Beckstein).

  * _adk\_dims_      Trajectory of a macromolecular transition of the enzyme adenylate kinase between a closed and an open conformation. The simulation was run in [CHARMM](http://www.charmm.org) c35a1.
  * _adk\_oplsaa_    Ten frames from the first 1 ns of a equilibrium trajectory of AdK in water with Na+ counter ions. The OPLS/AA forcefield is used with the TIP4P water model. The simulation was run with [Gromacs](http://www.gromacs.org) 4.0.2.


## References ##

  * O. Beckstein, E.J. Denning, J.R. Perilla and T.B. Woolf, Zipping and Unzipping of Adenylate Kinase: Atomistic Insights into the Ensemble of Open-Closed Transitions. J Mol Biol 394 (2009), 160--176, doi:[10.1016/j.jmb.2009.09.009](http://dx.doi.org/10.1016/j.jmb.2009.09.009)


# Writing test cases #

The tests are in a separate package, together with any data files required for running the tests (see [Issue 87](https://code.google.com/p/mdanalysis/issues/detail?id=87) for details). Whenever you _add a new feature_ to the code you _should also add a test case_ (ideally, in the same git commit so that the code and the test case are treated as one unit).

The unit tests use the [unittest module](http://docs.python.org/library/unittest.html) together with [nose](http://somethingaboutorange.com/mrl/projects/nose/0.11.3/index.html). See the examples in the [MDAnalysisTests](http://code.google.com/p/mdanalysis/source/browse/#git%2Ftestsuite%2FMDAnalysisTests) package.

The [SciPy testing guidelines](http://projects.scipy.org/numpy/wiki/TestingGuidelines#id11) are a good howto for writing test cases, especially as we are directly using this framework (imported from numpy).

Conventions for MDAnalysis
  * Test input data is stored in  [MDAnalysisTests/data](http://code.google.com/p/mdanalysis/source/browse/#git%2Ftestsuite%2FMDAnalysisTests%2Fdata).
    * Keep files small if possible; for trajectories 10 frames or less are sufficient.
    * Add the file name of test data files to [MDAnalysisTests/datafiles.py](http://code.google.com/p/mdanalysis/source/browse/testsuite/MDAnalysisTests/datafiles.py) (see the code for details).
    * Add the file(s) or a glob pattern to the `package_data` in [setup.py](http://code.google.com/p/mdanalysis/source/browse/package/setup.py); otherwise the file will not be included in the python package.
    * If you use data from a published paper then add a reference to _this wiki page_ and the doc string in [MDAnalysisTests/\_\_init\_\_.py](http://code.google.com/p/mdanalysis/source/browse/testsuite/MDAnalysisTests/__init__.py).
  * Tests are currently organized by top-level module. Each file containing tests must start with `test_` by convention (this is how nose/unittest works). Tests itself also have to follow the appropriate naming conventions. See the docs above or the source.
  * Tests that take longer than 3 seconds to run should be marked `@slow` (see e.g. the XTC tests in [MDAnalysisTests/test\_coordinates.py](http://code.google.com/p/mdanalysis/source/browse/testsuite/MDAnalysisTests/test_coordinates.py#1078). They will only be run if `labels="full"` is given as an argument to the `test()` function.
  * Add a test for
    * new functionality
    * fixed issues (typically named `test_IssueXX` or referencing the issue in the doc string (to avoid regression)
    * anything you think worthwhile – the more the better!


# Changes with releases #

The way we organized the unit tests changed between releases. The procedure for the current release is detailed at the very top of the page. The following list is for historical reference and in case you ever want to go back to a previous release.

  1. since **0.7.5**: tests _and_ data are together in package **MDAnalysisTests**. See [Issue 87](https://code.google.com/p/mdanalysis/issues/detail?id=87) for details.
  1. release **0.7.4**: tests are in **MDAnalysis** and data is in **MDAnalysisTestData** (for MDAnalysis == 0.7.4). To install [MDAnalysisTestData](MDAnalysisTestData.md) download the `MDAnalysisTestData-0.7.4.tar.gz` from the [Download](http://code.google.com/p/mdanalysis/downloads/list) section or try
```
easy_install http://mdanalysis.googlecode.com/files/MDAnalysisTestData-0.7.4.tar.gz
```
  1. release **0.6.1** to **0.7.3**: tests and data were included with **MDAnalysis**
  1. release **0.4** to **0.6.0**: no tests included