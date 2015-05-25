# Changes #

**0.7.5** release date: 02/11/12

MDAnalysis can now be found on [PyPI (MDAnalysis)](http://pypi.python.org/pypi/MDAnalysis), allowing simple installation
from the internet. Metadata was added to setup to facilitate PyPI
upload and pages on the wiki describe how to do this.
In addition, [Debian/Ubuntu packages](http://code.google.com/p/mdanalysis/wiki/Install#Installing_using_binary_packages_%28for_Ubuntu/Debian_users%29) are also available.

Note that in order to run UnitTests one needs the separate package [MDAnalysisTests](MDAnalysisTests.md) (with the same release number 0.7.5).

## Enhancements ##

  * new method OtherWriter() for trajectory readers to generate a    writer for any format that has been initialised for the common   basic values
  * new simple Residue.chi1\_selection() selection
  * new distances.between() function (EXPERIMENTAL)
  * support LAMMPS non-standard DCD files ([Issue 84](https://code.google.com/p/mdanalysis/issues/detail?id=84); EXPERIMENTAL)
  * read and write multi-frame PDB files ([Issue 77](https://code.google.com/p/mdanalysis/issues/detail?id=77); EXPERIMENTAL)
  * extend the PDB parsing, support CONECT and REMARK entries
  * new GNM-based trajectory analysis module ([Issue 90](https://code.google.com/p/mdanalysis/issues/detail?id=90))
  * Read/Write velocities with TRR, new attribute Atom.velocity and AtomGroup.velocities() ([Issue 91](https://code.google.com/p/mdanalysis/issues/detail?id=91))
  * hydrogen bond analysis detects a range of GLYCAM atom types    and utils.convert\_aa\_code will also accept GLYCAM-style residue    names ([Issue 92](https://code.google.com/p/mdanalysis/issues/detail?id=92))
  * XYZ reader: can set timestep ([Issue 92](https://code.google.com/p/mdanalysis/issues/detail?id=92))

## Changes ##

  * The UnitTests are now integrated with the separate test data in a  separate Python package named [MDAnalysisTests](MDAnalysisTests.md); to run the tests     for 0.7.5 one will need MDAnalysisTests-0.7.5 ([Issue 87](https://code.google.com/p/mdanalysis/issues/detail?id=87)).
  * install a range of analysis dependencies right away: networkx,    biopython, GridDataFormats (usually all painless); leave scipy and    matplotlib to the user and the local package manager
  * When writing a trajectory and converting units, effectively a copy of the timestep is made and the in-memory timestep is not altered. In his way, analysis after writing a frame will still see the coordinates in MDAnalysis units and not converted units.

## Fixes ##

  * analysis.align.rms\_fit\_traj(): can output fitted trajectory to any    supported format not just the input format
  * fixed ProgressMeter: default format string was broken
  * fixed: ResidueGroup and SegmentGroup indexing (did not work with    numpy.int64 etc) and now raise TypeError if it does not fit
  * fixed HydrogenBondingAnalysis backbone donor list: had C but    should have been O (this was supposed to be fixed with [r849](https://code.google.com/p/mdanalysis/source/detail?r=849) in    0.7.4 but a typo crept in). **NOTE: analysis might have produced    partially wrong results.**
  * fixed: dihedral() and other methods using core.util.angle() sometimes    returned nan instead of +pi or -pi
  * fixed: writing a trajectory from chained CRD files gave garbage    coordinates ([Issue 81](https://code.google.com/p/mdanalysis/issues/detail?id=81))
  * fixed: support files for docs are now in included in the source    distribution (thanks to Sebastien Buchoux; [Issue 82](https://code.google.com/p/mdanalysis/issues/detail?id=82))
  * fixed: core.util.iterable() would wrongly detect unicode strings    as "iterable"; this lead the Reader autodections and then the    ChainReader fail with "Runtime Error: Maximum recursion depth    exceeded" for single filenames provided as a unicode string.
  * fixed: HBond analysis pickling of tables ([Issue 92](https://code.google.com/p/mdanalysis/issues/detail?id=92))

# Authors #
orbeckst, sebastien.buchoux, jandom, hallben, lukasgrossar