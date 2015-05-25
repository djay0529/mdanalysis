## CHANGELOG ##

  * 0.7.6
  * released [4th July](http://en.wikipedia.org/wiki/Independence_Day_%28United_States%29) 2012

> ### Enhancements ###

  * GRO file velocities may be accessed as AtomGroup.velocities() or Atom.velocity ([Issue 102](https://code.google.com/p/mdanalysis/issues/detail?id=102))
  * PrimitivePDBReader can be sliced
  * AMBER NetCDF (binary trajectory) reader and writer, supporting coordinates and velocities; requires netcdf4-python ([Issue 109](https://code.google.com/p/mdanalysis/issues/detail?id=109))
  * additional attributes and methods for AtomGroup to consolidate the interface to the Timestep: attribute 'positions' and 'get\_positions()' can be used instead of the 'coordinates()' method. get/set methods for both positions and velocities.
  * almost all Readers now support some form of slicing; unsupported slicing operations will raise a TypeError
  * additional analysis for Nucleic Acid order parameters (MDAnalysis.analysis.nuclinfo)
  * AMBER TOPParser now able to do both amber10 and amber12 formats ([Issue 100](https://code.google.com/p/mdanalysis/issues/detail?id=100))

> ### Changes ###

  * HydrogenBondAnalysis: multiple enhancements and changes ([Issue 103](https://code.google.com/p/mdanalysis/issues/detail?id=103))
    * many new analysis functions (see docs)
    * run() does not return the results anymore; results are simply stored as attribute timeseries (similar to other analysis tools)
    * only write per-frame debugging messages to the logfile when the new verbose keyword is set to True
    * more reliable detection of hydrogens bonded to heavy atoms
    * remove duplicate hydrogen bonds from the output
  * removed CHO and EAM (formyl and ethanol termini of gA in CHARMM) from the set of residues recognized as protein (collision with commonly used CHO for cholesterol)
  * PrimitivePDBWriter: special segid SYSTEM is translated to empty chainID
  * In order to write multi frame PDB files, the multiframe=True keyword must be supplied or use the MultiPDBWriter
  * empty AtomGroup can be constructed or can result from a selection without matches; it does **not** raise NoDataError anymore ([Issue 12](https://code.google.com/p/mdanalysis/issues/detail?id=12))
  * all single frame readers denote the first (and only) frame as frame number 1 (i.e. ts.frame == 1); it used to be 0 but 1 is consistent with the way this is is handled with real trajectories
  * requires Biopython >= 1.51 (fixes for [Issue 112](https://code.google.com/p/mdanalysis/issues/detail?id=112) and [Issue 113](https://code.google.com/p/mdanalysis/issues/detail?id=113))
  * Atom.type is always stored as a string.

> ### Fixes ###

  * HydrogenBondAnalysis: NH1 and NH2 were not recognized
  * GROWriter: enforce maximum resname and atomname length of 5 chars
  * Universe.load\_new() raised a NameError (thanks to JiyongPark.77)
  * fixed [Issue 105](https://code.google.com/p/mdanalysis/issues/detail?id=105) (trajectory snapshots could not be written to PDB)
  * fixed [Issue 107](https://code.google.com/p/mdanalysis/issues/detail?id=107) (NAMD/VMD space delimited PSF files can be autodetected and read); important when using CGENFF atom types (thanks to JiyongPark.77 for initial patch)
  * fixed [Issue 101](https://code.google.com/p/mdanalysis/issues/detail?id=101) (could not write single frame to trr file)
  * fixed: permissive=True flag was ignored in Universe and hence the PrimitivePDBReader was always selected even if the Biopython one was desired
  * fixed [Issue 112](https://code.google.com/p/mdanalysis/issues/detail?id=112) (used removed Biopython constructs in MDAnalysis.analysis.align.fasta2select; thanks to francesco.oteri for a test case and fix)
  * fixed failing 'type' selection for topology formats that read an atom type as an integer (such as the AMBER parser)
  * fixed [Issue 111](https://code.google.com/p/mdanalysis/issues/detail?id=111) (NAN in pycpqrot and RMSD calculation)
  * fixed [Issue 113](https://code.google.com/p/mdanalysis/issues/detail?id=113) (replaced outdated Biopython to call ClustalW)

## Authors ##
orbeckst, joshua.adelman, andy.somogyi, tyler.je.reddy, lukas.grossar, denniej0