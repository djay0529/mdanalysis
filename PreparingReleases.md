This document outlines how to manage the MDAnalysis sources and how to prepare distribution packages. See also DevelopmentWorkflow for new ideas how to organize codebase and workflow.



# Release policy and release numbering #
We use a **MAJOR**.**MINOR**.**PATCH** scheme to label releases. We adhere to the idea of [semantic versioning](http://semver.org/) (semantic versioning was introduced with release 0.9, see [Issue 200](https://code.google.com/p/mdanalysis/issues/detail?id=200)): Given a version number **MAJOR.MINOR.PATCH**, we increment the:

  * **MAJOR** version when we make **incompatible API changes**,
  * **MINOR** version when we **add functionality** in a backwards-compatible manner, and
  * **PATCH** version when we make backwards-compatible **bug fixes**.

However, as long as the **MAJOR** number is **0** (i.e. the API has not stabilized), even **MINOR** increases _may_ introduce incompatible API changes. As soon as we have a 1.0.0 release, the public API can only be changed in a backward-incompatible manner with an increase in MAJOR version.

Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

The [CHANGELOG](http://code.google.com/p/mdanalysis/source/browse/CHANGELOG) lists important changes for each release.

_MAJOR_,  _MINOR_, _PATCH_  number are integers that increase monotonically. Pre-releases are labeled with the postfix -rc1, -rc2, etc.

The **release number** is set in [setup.py](http://code.google.com/p/mdanalysis/source/browse/setup.py) _and_ in `MDAnalysis.__version__` (MDAnalysis/init.py), e.g.
```
RELEASE = '0.7.5'
```

**While the code is in development** (i.e. whenever we are not preparing a release!) the release number gets the suffix **-devel**, e.g.
```
RELEASE = '0.7.6-devel'
```
so that people using the [development branch](DevelopmentBranch.md) from the sources see immediately that this is not a final release. For example, "0.7.6-devel" is the state _before_ the 0.7.6 release.

# git check-ins #
  * Check in sets of changes via [git](git.md). Ideally, all changes in one revision should have a common theme. If you implemented two rather different things (say, one bug fix and one new feature) then split your check in into two.
  * _Always_ add a descriptive comment (feel free to be verbose).


# Preparing distribution tar balls #
## Preparing ##
  * Update local repository (`git pull`) and make sure that there are no conflicts.
  * Update [CHANGELOG](http://code.google.com/p/mdanalysis/source/browse/CHANGELOG) with the release number and summarize important changes. Add all authors that contributed to this release. See the file itself for guidelines and formatting.
  * Commit updated tree `git commit`
  * Set `RELEASE` in `setup.py`  and `MDAnalysis.__version__` (MDAnalysis/init.py); note that there is a `setup.py` in the MDAnalysis package _and_ the [MDAnalysisTests](MDAnalysisTests.md) testsuite!). There's a script `maintain/change_release.sh` that typically does the job:
```
./maintainer/change_release.sh 0.9.0
```
> Make sure to add and commit the changes!
  * Test if the distribution (MDAnalysis and MDAnalysisTests) builds successfully and run the UnitTests:
```
./maintainer/run_tests.sh
```
> Fix code so that all tests are passed (except for known failures). Note that if you worked on any of the **[Cython](http://cython.org/)** code then you should check that the new code was also compiled (monitor the output from `python setup.py build`).

> <font size='1'>
<blockquote>Note on Cython code (see <a href='https://code.google.com/p/mdanalysis/issues/detail?id=85'>Issue 85</a> for details):<br>
<ul><li>If no Cython, <code>setup.py</code> will use C-code that was previously generated; of you updated any <code>*.pyx</code> files then you <i>must</i> have <a href='http://cython.org/#download'>Cython</a> installed (but you probably new this already).<br>
</li><li>If Cython if installed, it is used to compile extension and <code>.pyx</code> source files are used instead of <code>.c</code> files.<br>
</li><li>From there, <code>.pyx</code> files are converted to <code>.c</code> files if they are newer than the already present <code>.c</code> files <i>or</i> if the <code>--force</code> flag is set (e.g <code>setup.py build --force</code>).</li></ul></blockquote>

<blockquote>Therefore, as a developer you should (1) have Cython installed and (2) only use <code>--force</code> if you are positive that you want to regenerate all <code>.c</code> files.<br>
</font></blockquote>

  * commit any changes and fixes that were necessary to make it work with `git commit`
  * Build a _source distribution_ tar ball with
```
# MDAnalysis
cd package
rm -f setup.cfg
python setup.py sdist

# MDAnalysisTests
cd ../testsuite
python setup.py sdist
```
> This builds the distribution under `package/dist/MDAnalysis-MAJOR-MINOR-PATCH.tar.gz` and `testsuite/dist/MDAnalysisTests-MAJOR-MINOR-PATCH.tar.gz`.


## Testing ##
Unpack the distribution in a tmp directory and try to build it:
```
mkdir tmp && cd tmp
tar -zxvf ../dist/MDAnalysis-0.7.5.tar.gz
cd MDAnalysis-0.7.5
python setup.py build --build-lib=.
```
The new test cases should be in your system as developer installation (from the previous step) so you should be able to run the [UnitTests](UnitTests.md) (run `python` from the _same_ directory):
```
>>> import MDAnalysis.tests
>>> MDAnalysis.tests.test(label='full', extra_argv=['--exe'])
```
(this imports the module compiled in `./MDAnalysis`.)

The above should work at least on Linux and Mac OS X. If it fails then go back and fix things and _do not release._

If everything works then we are now reasonably confident that a user can also compile and use the package so we push the source code for the release to the master repository. Push it to the master with
```
git push googlecode master
```
(or similar command).



## Tagging ##
The distribution is now ready. Tag it in [git](git.md):
```
git tag -m 'release 0.7.5 of MDAnalysis and MDAnalysisTests' release-0.7.5
git push --tags googlecode master
```
The **tag format** is the string **release-** followed by **major.minor.patch**.

## Upload to PyPi ##
  * upload the source distribution tar balls of MDAnalysis and MDAnalysisTests to the python package index (e.g. using `python setup.py sdist bdist bdist_egg upload`)
  * [build the docs and upload to PyPi](#Update_docs_on_PyPI.md)
  * make the new tar ball a _featured_ release so that it shows up on the front page (and _unfeature_ any older releases).
  * provide a short description (a condensed version of the `CHANGELOG`)

## Update/create a release page on the wiki ##
Create a ReleaseXYZ wiki page, modelled after e.g. [Release062](Release062.md) (using the [CHANGELOG](http://code.google.com/p/mdanalysis/source/browse/CHANGELOG) as a reference). Also add it to the [TableOfContents](TableOfContents.md).

## Update docs on PyPI ##
Since 0.7.4 there's also a [MDAnalysis page on PyPi](http://pypi.python.org/pypi/MDAnalysis/) (the Python Package Index). The download page points to the Googlecode page but the metadata needs to be updated:
```
python setup.py register
```
will do this (although you need to be the PyPI package owner or a maintainer—contact orbeckst in order to be added).

For full releases we can also have the documentation at http://packages.python.org/MDAnalysis/ . It is updated by _zipping_ the html docs and uploading manually:
```
cd doc/html
zip -r ../pypidoc.zip *
```
Then _upload documentation_ (the file `pypidoc.zip`) via the [MDAnalysis: Edit](http://pypi.python.org/pypi?%3Aaction=pkg_edit&name=MDAnalysis) page.


# Test data #

UnitTests and test data are maintained in the same git repository as the MDAnalysis package. Tests are checked in together with code (for details see [Issue 87](https://code.google.com/p/mdanalysis/issues/detail?id=87), UnitTests and [MDAnalysisTests](MDAnalysisTests.md)).

The library, **MDAnalysis** and the tests, **MDAnalysisTests** share the same release number: whenever there's a new MDAnalysis there will be a corresponding MDAnalysisTests. Therefore, the two packages are maintained and built together (see above).