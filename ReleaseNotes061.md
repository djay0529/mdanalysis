# Availability #

You can download the tar ball for the **MDAnalysis 0.6.1** from [MDAnalysis-0.6.1.tar.gz](http://code.google.com/p/mdanalysis/downloads/detail?name=MDAnalysis-0.6.1.tar.gz) or check out
```
svn checkout http://mdanalysis.googlecode.com/svn/branches/release-0-6-1 mdanalysis-0.6.1
```

# Major additions #

It is now possible to **read Gromacs xtc and trr trajectories** using the native Gromacs xdrfile library (included). You can provide a pdb file instead of a psf to define the atoms (although this does not generate the bonds yet).

There are also test cases included for **automated [unit tests](UnitTests.md)**. The test trajectories were taken from one of our recent papers (see docs for details). This makes the tar ball much bigger than before (16 MiB) but allows you to check the functionality by doing
```
import MDAnalysis.tests
MDAnalysis.tests.test()
```

(In later releases we will add additional test cases and possibly factor the testing framework out of the standard package.)


# Changes #

## CHANGELOG ##

  * 0.6.1 release
  * can build a simple Universe from a PDB file (FIXES [Issue 11](https://code.google.com/p/mdanalysis/issues/detail?id=11))
  * can read Gromacs XTC and TRR files (FIXES [Issue 1](https://code.google.com/p/mdanalysis/issues/detail?id=1)) but no Timeseries or Collections yet for those formats
  * removed `Universe.load_new_dcd()` and `Universe.load_new_pdb()` --- use the generic `Universe.load_new()` (_MIGHT BREAK OLD CODE_)
  * removed deprecated `Universe._dcd` attribute (_MIGHT BREAK OLD CODE_)
  * FIXED bug in PDB.PDBWriter and CRD.CRDWriter
  * use SloppyPDB in order to cope with large PDB files
  * `core.distances.self_distance_array()` is now behaving the same way as `distance_array()`
  * defined Trajectory API (see [MDAnalysis/coordinates/\_\_init\_\_.py](http://code.google.com/p/mdanalysis/source/browse/MDAnalysis/coordinates/__init__.py))
  * renamed `_dcdtest` to `dcdtimeseries` (will not affect old code)
  * unit tests added (still need more test cases)


## Source code ##
We are also changing the develop workflow. From now on, most of the development will be done on the svn **trunk** so in order to follow development so please do not use  /svn/branches/development-UNSTABLE-orbeckst anymore but simply do a
```
svn checkout http://mdanalysis.googlecode.com/svn/trunk/ mdanalysis
```

(Developers should remove their old mdanalysis directory and do
```
svn checkout --username USER https://mdanalysis.googlecode.com/svn/trunk/ mdanalysis    
```
to contribute to the main development line. See also DevelopmentWorkflow )

We'll try to keep the trunk reasonably clean (using unit tests). Releases will be found on /svn/branches from now on.

Please report problems through the [Issue Tracker](http://code.google.com/p/mdanalysis/issues/list) or the mailing list.