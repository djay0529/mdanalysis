# CHANGELOG #

  * 0.6.0 release
  * added KDTree and Neighbour searching code from [Biopython](http://biopython.org) for faster distance selections: used for AROUND selections; POINT is using distance matrix by default as this is  faster. This can be configured with `core.flags`
  * `core.flags` infrastructure to tweak behaviour at run time
  * updated LICENSE file with Biopython License
  * some selections for nucleic acids
  * completely reorganized directory structure to make enhancements easier to incorporate
  * FIXED (partial): [Issue 18](https://code.google.com/p/mdanalysis/issues/detail?id=18) (`Timeseries` from a universe.segID selection, reported by lordnapi)
  * FIXED: [Issue 19](https://code.google.com/p/mdanalysis/issues/detail?id=19) (`Timeseries` collections were broken, reported by Jiyong)
  * can write single frames as pdb or crd (`AtomGroup` gained the `write()` method)
  * improved installation
    * EasyInstall (setuptools) support (FIXED [Issue 16](https://code.google.com/p/mdanalysis/issues/detail?id=16))
    * better instructions in [INSTALL](Install.md)
    * slightly better handling of the configuration of the fast linear algebra libs via the `setup.cfg` file