# Major Changes #
  * Amber support ([PRMTOP](http://ambermd.org/formats.html#parm) topology file reader and [TRJ](http://ambermd.org/formats.html#trajectory) trajectory file reader...netcdf4 format NOT yet included)
  * [PQR](http://www.poissonboltzmann.org/file-formats/biomolecular-structurw/pqr) file reading support ([Issue 54](https://code.google.com/p/mdanalysis/issues/detail?id=54))

# Changes #

  * new `analysis.contacts.ContactAnalysis1` class that supports a native contact analysis between arbitrary groups
  * new `analysis.density` module for the creation and analysis of volume data (uses the [GridDataFormats](http://pypi.python.org/pypi/GridDataFormats/) package)
  * new examples (e.g. peptide helix clustering in a membrane)
  * fixed [Issue 58](https://code.google.com/p/mdanalysis/issues/detail?id=58) (`align.rms_fit_trj`; fix reported by Joshua Adelman)
  * fast RMSD aligner based on [Douglas Theobald's QCP method](http://theobald.brandeis.edu/qcp/) for calculating the minimum RMSD between two structures and determining the optimal least-squares rotation matrix; replaces the slower previous code (implemented by Joshua Adelman from his [pyqcprot](https://github.com/synapticarbors/pyqcprot) package)
  * **deprecated `core.rms_fitting.rms_rotation_matrix()` and scheduled for removal in 0.8** (the QCP rotationmatrix calculation is much faster)
  * uses **[cython](http://cython.org/)** instead of **pyrex**

# Authors #

denniej0, orbeckst, jandom, tyler.je.reddy, Joshua Adelman