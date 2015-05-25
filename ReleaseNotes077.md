## CHANGELOG ##

  * 0.7.7
  * released [24 December](http://en.wikipedia.org/wiki/Christmas_Eve), 2012

> ### Enhancements ###

  * `multithreaded distance_array()` ([Issue 80](https://code.google.com/p/mdanalysis/issues/detail?id=80), experimental); see the new `core.parallel.distance` module
  * `MDAnalysis.analysis.rms` for simple RMSD analysis
  * format of input coordinates can be set as `(filename, format)` tuples ([Issue 76](https://code.google.com/p/mdanalysis/issues/detail?id=76))
  * new `AtomGroup.asphericity()` and `AtomGroup.shapeParameter()` methods to compute shape descriptors.
  * access to forces (AtomGroup.forces with `get_forces()` and `set_forces()`; the default unit for force is kJ/(mol\*A) and it is automatically converted from/to native). Currently, only the TRR Reader/Writer support forces.
  * all element masses
  * logger reports current version when starting

> ### Fixes ###

  * fixed [Issue 115](https://code.google.com/p/mdanalysis/issues/detail?id=115) (GROReader now uses fixed-column width format to read GRO files)
  * fixed [Issue 116](https://code.google.com/p/mdanalysis/issues/detail?id=116) (Failed to write AMBER netcdf trajectory from AtomGroup)
  * fixed [Issue 117](https://code.google.com/p/mdanalysis/issues/detail?id=117) (could not write Gromacs XTC/TRR from AMBER netcdf)
  * fixed [Issue 120](https://code.google.com/p/mdanalysis/issues/detail?id=120) (DCDWriter: wrote wrong unitcell information)
  * fixed [Issue 121](https://code.google.com/p/mdanalysis/issues/detail?id=121) (PSFParser would fail with IndexError for files without SEGID)
  * [Issue 122](https://code.google.com/p/mdanalysis/issues/detail?id=122) (made installation of netCDF4 library optional, which means that users of the AMBER netcdf Reader/Writer will have to manually install the library and its dependencies netcdf and HDF5, see the wiki page on [netcdf](https://code.google.com/p/mdanalysis/wiki/netcdf))

## Authors ##
danny.parton, jandom, orbeckst, jjlights03, jphillips, naveen.michaudagrawal, andy.somogyi