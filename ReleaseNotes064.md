# Changes #

  * release 0.6.4
  * GRO writer added
  * fixed XTC writer ([Issue 38](https://code.google.com/p/mdanalysis/issues/detail?id=38))
  * convert box representations ([Issue 37](https://code.google.com/p/mdanalysis/issues/detail?id=37))
  * primitive PDB parser added (slightly faster and ignores correctness of resids, atomnames etc but reads CRYST1 into unitcell)
  * Universe gained the _permissive_ flag to switch on the primitive PDB parser/reader
  * Simple 'chained reader' which enables a Universe to transparently read a list of trajectory files ([Issue 39](https://code.google.com/p/mdanalysis/issues/detail?id=39)).
  * Additional methods for _AtomGroup_: numberOfResidues(), resids(), resnames()
  * analysis
    * new bilayer analysis script for membrane composition on a per-leaflet basis ([examples/membrane-composition.py](http://code.google.com/p/mdanalysis/source/browse/branches/release-0-6-4/examples/membrane-composition.py))
    * renamed examples/leaflet.py to membrane-leaflets.py


# Authors #

orbeckst, danny.parton