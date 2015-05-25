The 0.7.4 release of MDAnalysis has _considerably shrunk in size_: Instead of a 20 MB tar file it is now about 1 MB because all the trajectories and files for testing have been moved into a separate package, [MDAnalysisTestData](MDAnalysisTestData.md) (file [MDAnalysisTestData-0.7.4.tar.gz](http://code.google.com/p/mdanalysis/downloads/detail?name=MDAnalysisTestData-0.7.4.tar.gz)), which is to be installed separately. For more details see [Issue 28](https://code.google.com/p/mdanalysis/issues/detail?id=28) and [MDAnalysisTestData](MDAnalysisTestData.md).

# Changes #

0.7.4 release

## Enhancements ##

  * [Universe()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/core/AtomGroup.html?highlight=universe#MDAnalysis.core.AtomGroup.Universe) got new keywords _topology\_format_ and _format_ to allow the user to specify the file formats instead of deriving it from the   extensions (does not work with "chained" files at the moment); thanks to Michael Lerner for the suggestion
  * Chain trajectory reader allows frame indexing.
  * [Issue 75](https://code.google.com/p/mdanalysis/issues/detail?id=75): additional donors and acceptors keywords for H-bond analysis
  * structural alignment functions [alignto()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/align.html?highlight=alignto#MDAnalysis.analysis.align.alignto) and [rms\_fit\_traj()](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/analysis/align.html?highlight=rms_fit_trj#MDAnalysis.analysis.align.rms_fit_trj) can also take a list of selection strings in order to define atom groups with fixed atom order and [alignto()](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/analysis/align.html?highlight=alignto#MDAnalysis.analysis.align.alignto) preserves the order of supplied AtomGroups for the special select values _"all"_ and _None_.
  * new `set_*` methods for AtomGroup allows changing of Atom attributes for all members of the group (such as the _segid_) (EXPERIMENTAL)
  * new Atom and Residue attribute `resnum` that can be used to store the canonical PDB residue number (EXPERIMENTAL)

## Fixes ##

  * fixed [Issue 74](https://code.google.com/p/mdanalysis/issues/detail?id=74) (bug in AMBER topology parser which would show up for certain numbers of input lines; thanks to htaoyu1)
  * fix for [Issue 48](https://code.google.com/p/mdanalysis/issues/detail?id=48) (sparse contact\_matrix in [distances.py](http://code.google.com/p/mdanalysis/source/browse/MDAnalysis/analysis/distances.py) was slow as written in pure Python; was optimized in C code using [scipy.weave](http://www.scipy.org/Weave))
  * HydrogenBondingAnalysis: donor atom name CO --> O (proper backbone oxygen) and acceptor NH -> N; without the fix one misses most of the backbone H-bonds
  * [alignto()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/align.html?highlight=alignto#MDAnalysis.analysis.align.alignto) and [rms\_fit\_traj()](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/analysis/align.html?highlight=rms_fit_trj#MDAnalysis.analysis.align.rms_fit_trj): order of mobile and reference selection was reversed when supplied as a tuple `(sel1, sel2)`

## Changes ##

  * replaced `analysis.util.progress_meter()` with class [core.log.ProgressMeter](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/core/log.html?highlight=progressmeter#MDAnalysis.core.log.ProgressMeter)
  * [Issue 28](https://code.google.com/p/mdanalysis/issues/detail?id=28): split off test data trajectories and structures from `MDAnalysis/tests/data` into separate package [MDAnalysisTestData](MDAnalysisTestData.md), which is required to run the UnitTests from release 0.7.4 onwards. Numbering matches the earliest MDAnalysis release for which the data is needed. Any later releases of MDAnalysis will also use these test data unless a `MDAnalysisTestData` package with a higher release number is available


# Authors #
orbeckst, dcaplan, jandom