Release 0.7.2 of MDAnalysis (31 March 2011).

# Changes #

  * Release 0.7.2
  * _NOTE_: minimum Python version required is 2.5 (since 0.6.3)

## Enhancements ##
  * loading from a PDB sets segid to the chain id if it exists
  * [PrimitivePDBWriter](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/PDB.html?highlight=primitivepdbwriter#MDAnalysis.coordinates.PDB.PrimitivePDBWriter) uses first letter of segid as PDB chain id
  * aliased `segment.name` to `segment.id`
  * new method [AtomGroup.bbox()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/core/AtomGroup.html?highlight=bbox#MDAnalysis.core.AtomGroup.AtomGroup.bbox) that returns the orthorhombic bounding box
  * enhancements of the [analysis.density](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/density.html) module (build density from B-factors with [density\_from\_PDB](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/analysis/density.html?highlight=bfactor#MDAnalysis.analysis.density.density_from_PDB))
  * PQR radius is now an attribute of [Atom](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/core/AtomGroup.html#MDAnalysis.core.AtomGroup.Atom); [AtomGroup.radii()](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/core/AtomGroup.html#MDAnalysis.core.AtomGroup.AtomGroup.radii) returns the radii as a NumPy array; internally B-factor has also become an attribute of each Atom.
  * recognise many more OPLS/AA and Amber residue names as "protein"
  * recognise more atom masses (taken from CHARMM27 and Gromacs) and atom types (from CHARMM, Amber, OPLS, GROMOS) and moved masses and types into new module [topology.tables](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/topology/tables.html); the type recognition is still incomplete but can be easily enhanced in tables
  * [analysis.align](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/align.html): convenience functions [rotation\_matrix()](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/analysis/align.html#MDAnalysis.analysis.align.rotation_matrix) and [alignto()](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/analysis/align.html#MDAnalysis.analysis.align.alignto)
  * TrajectoryReader gained [Writer()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/base.html#MDAnalysis.coordinates.base.Reader.Writer) method which returns an appropriate TrajectoryWriter instance that can be used for processing this trajectory (enhancement of the Trajectory API); if no Writer is known then a NotImplementedError is raised
  * [doc](http://mdanalysis.googlecode.com/git/doc/html/index.html) improvements


## Fixes ##
  * installation: removed dependency on [Cython](http://cython.org); developer should use [setup\_developer.py](http://code.google.com/p/mdanalysis/source/browse/setup_developer.py) instead of [setup.py](http://code.google.com/p/mdanalysis/source/browse/setup.py) ([Issue 66](https://code.google.com/p/mdanalysis/issues/detail?id=66))
  * Fixed a problem with the strict [PDBReader](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/PDB.html?highlight=pdbreader#MDAnalysis.coordinates.PDB.PDBReader): raised exception when the pdb did not contain a segid
  * Support for PDBs with 4 character resnames and segID output when writing ([Issue 63](https://code.google.com/p/mdanalysis/issues/detail?id=63)) --- makes the (default) PrimitivePDBReader/Writer more suitable for NAMD/CHARMM but breaks [strict PDB standard](http://www.wwpdb.org/documentation/format32/v3.2.html). If you need full PDB reading capabilities, use the strict [PDBReader](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/PDB.html?highlight=pdbreader#MDAnalysis.coordinates.PDB.PDBReader) (i.e. use `Universe(..., permissive=False)`).
  * fixed bug in (experimental) phi and psi selections
  * fixed bugs in reading of unit cells ([Issue 60](https://code.google.com/p/mdanalysis/issues/detail?id=60), [Issue 61](https://code.google.com/p/mdanalysis/issues/detail?id=61), [Issue 34](https://code.google.com/p/mdanalysis/issues/detail?id=34))
  * [universe.trajectory.delta](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/init.html?highlight=delta#id8) returns the full precision dt value instead of a value rounded to 4 decimals ([Issue 64](https://code.google.com/p/mdanalysis/issues/detail?id=64))
  * fixed bug in [DCDWriter](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/DCD.html?highlight=dcdwriter#MDAnalysis.coordinates.DCD.DCDWriter) (XTC->DCD was broken, [Issue 59](https://code.google.com/p/mdanalysis/issues/detail?id=59))



# Authors #

orbeckst, dcaplan, naveen.michaudagrawal