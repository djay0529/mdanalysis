This is a new major release of MDAnalysis. In line with our [release policy](PreparingReleases.md), it introduces new features at all levels of the code and removes old legacy code in favour for a more consistent (and hopefully more intuitive) API. These changes **may break old scripts using MDAnalysis**. Major effort went into making the AtomGroup work in all situations and allow users to access segments/residues/atoms from any Atom, AtomGroup, Residue, or Segment.

Trajectory writing has been streamlined (e.g. writing of selections can be done by just supplying the appropriate AtomGroup to the `Writer.write()` method).

A number of bugs have been fixed and the coverage of UnitTests expanded. New scripts and improvements to the MDAnalysis.analysis module should make it easier for users to adapt MDAnalysis to their own use cases.

If you have old code then it is worthwhile looking at the Changes below in more detail as it lists changes that are known to be incompatible with pre-0.7 code.

We hope that the improvements in this release make it worthwhile to update your old code and encourage you to report any problems via the mailing list and/or the [Issue Tracker](http://code.google.com/p/mdanalysis/issues/list).


# Changes #

major release 0.7.0: (includes changes that can BREAK BACKWARDS COMPATIBILITY)

## Removed _all deprecated_ code ##
In particular, the following methods are gone or are not found in the previous place anymore:
  * `AtomGroup.principleAxes` ([Issue 33](https://code.google.com/p/mdanalysis/issues/detail?id=33))
  * `DCD.DCDReader.dcd_header()` and `DCD.DCDWriter.dcd_header()` (use `_dcd_header()`)
  * `Universe.dcd` (and `Universe.xtc`, `Universe.trr`...) --- from now on only `Universe.trajectory` is supported. **WILL BREAK LEGACY CODE!**
  * removed the following packages from top-level MDAnalysis  name space:
    * `AtomGroup`, `Selection`: import them from `MDAnalysis.core` if really needed (e.g. `import MDAnalysis.core.AtomGroup`)
    * `distances`, `rms_fitting`: `import MDAnalysis.analysis.distances` or `import MDAnalysis.analysis.align.rms_fitting` (the actual modules still live in `MDAnalysis.core` but they might get moved in the future and bundled with `transformations`)
    * `from MDAnalysis import *` will only get `['Timeseries', 'Universe', 'asUniverse', 'Writer', 'collection']`
  * removed `copy` flag from `distance_array` and `self_distance_array`:  setting it to False would always give wrong results so there was no good reason keeping it around

## Trajectory I/O and file formats ##
  * improved trajectory writing
    * `MDAnalysis.Writer()` factory function that provides an appropriate writer for the desired file format
    * `Writer.write()` accepts a `Universe` or a arbitrary `AtomGroup` (e.g. from a selection); this is much more flexible than `Writer.write_next_timestep()`; the DCD/XTC/TRR writer also accepts a `Timestep` for its `write()` method.
  * CHARMM CRD coordinate files
    * CRDReader added (fixes [Issue 40](https://code.google.com/p/mdanalysis/issues/detail?id=40)) ... it will work for both standard and extended formats: NO special flags needed.
    * CRDWriter will now write extended crd files: NO special flags needed.
  * PDB format:    By default, PDB files are read with the PrimitivePDBReader and not	  the Bio.PDB reader anymore because the latter can drop atoms when	  they have the same name in a residue (which happens for systems	  generated from MD simulations) The PrimitivePDBReader copes just fine 	  with those cases (but does not handle esoteric PDB features such as	  alternative atoms and insertion codes that are not needed for	  handling MD simulation data).
    * The default behaviour of MDAnalysis  can be set through the flag `MDAnalysis.core.flag['permissive_pdb_reader']`.  The default is True.
    * One can always manually select the PDB reader by providing the _permissive_ keyword to Universe; e.g. `Universe(PDB, permissive=False)` will read the input file with the Bio.PDB reader. This might be  useful when processing true Protein Databank PDB files.

## Changes to `AtomGroup`s ##
  * AtomGroup
    * Indexing is made consistent with the way lists behave:
      1. indexing with integers returns a single `Atom`
      1. slicing always returns a new `AtomGroup`
      1. advanced slicing with a list or array returns a new `AtomGroup` (_NEW_, fixes [Issue 36](https://code.google.com/p/mdanalysis/issues/detail?id=36))
    * `AtomGroup` coordinates can be manipulated (`translate()`, `rotate()` and `rotateby()` methods; when appropriate, these methods can take `AtomGroup`s or arrays to determine coordinates)
    * **new attributes** `residues` and `segments` for `AtomGroup` to give access to the list of residue/segment objects of the group
    * new exception `NoDataError`; raised when creation of an empty `AtomGroup` is attempted (see also [Issue 12](https://code.google.com/p/mdanalysis/issues/detail?id=12))
    * **consistent representation of the Segment > Residue > Atom hierarchy** : all classes related to `AtomGroup` now expose the attributes `atoms`, `residues`, `segments` that provide access to groups of the corresponding objects
  * improvements to `Residue`, `ResidueGroup` and `Segment` classes; documented the _instant selector_ pseudo-attributes:
    * documented accessing residues from `Segment` as `Segment.r<resid>`; resid is 1-based -- _BREAKS OLD CODE_ that used the previous undocumented feature (which was 0-based)
    * added `SegmentGroup` class
    * can write from `Residue`, `ResidueGroup` and `Segment` ([Issue 46](https://code.google.com/p/mdanalysis/issues/detail?id=46))
    * residue name attribute of a `Segment` now consistently returns a `ResidueGroup` ([Issue 47](https://code.google.com/p/mdanalysis/issues/detail?id=47)) -- _MIGHT BREAK OLD CODE_
    * added documentation and examples in the doc strings
    * new special dihedral angle selections defined for `Residue` class to simplify analysis of backbone torsions (_experimental_)


## Other changes ##
  * whitespace is no longer required around parentheses for `selectAtoms()` strings but the old syntax with white space still works ([Issue 43](https://code.google.com/p/mdanalysis/issues/detail?id=43))
  * new `contact_matrix` method for calculating contacts ([Issue 30](https://code.google.com/p/mdanalysis/issues/detail?id=30)); for large (N > ~10000) coordinate arrays automatically switches to a method using a sparse matrix (slower)
  * more example scripts (e.g. for membrane analysis, trajectory writing, coordinate transformations)
  * fixed [Issue 51](https://code.google.com/p/mdanalysis/issues/detail?id=51) (distance\_array() did not properly check its input and wrong results could be returned if the input was a float64 and a float32 array)

## Experimental features ##

  * manipulation of coordinates through the `translate` and `rotateby` methods of an AtomGroup
  * a `Residue` can return dihedral angle selections for the protein phi, psi, and omega backbone angles


# Authors #
orbeckst, denniej0, tyler.je.reddy, danny.parton, joseph.goose