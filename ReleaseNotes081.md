This is a major new release of MDAnalysis, more than 1 year after [0.7.7](ReleaseNotes077.md). In addition to bug fixes it contains many new features (such as improvements in handling Gromacs data such as a fast random access trajectory reader and TPR reading capability), new analysis modules, and it introduces the MDAnalysis.visualization module, which collects code aiding in visualizing MD data.

There are a number of potentially backwards-incompatible [changes](#Changes.md), including a minimum requirement for Python 2.6 and some methods have become managed attributes.

From this release onwards, source code distributions are exclusively hosted on the Python Package index at https://pypi.python.org/pypi/MDAnalysis and pre-built packages for a range of distributions can be found at http://download.opensuse.org/repositories/home:/MDyNMR-FTW:/MDAnalysis/


## CHANGELOG ##

  * 0.8.1
  * released [1 April](http://en.wikipedia.org/wiki/April_Fools%27_Day), 2014

(Note that we effectively skipped [0.8.0](ReleaseNotes080.md).)

### Enhancements ###

  * Named selections can now be passed to selectAtoms ([Issue 174](https://code.google.com/p/mdanalysis/issues/detail?id=174))
  * (experimental) MDAnalysis.visualization namespace added along with 2D/3D streamplot modules & documentation for them
  * TRR file handling is now fully aware of missing coordinate/velocity/force information when reading and writing frames.
  * MDAnalysis.analysis.contacts.ContactAnalysis1 run() method    now allows trajectory slicing ([Issue 161](https://code.google.com/p/mdanalysis/issues/detail?id=161))
  * Merge AtomGroups into a new Universe ([Issue 157](https://code.google.com/p/mdanalysis/issues/detail?id=157))
  * TPR parser (currently limited to versions 58, 73 and 83 of the    Gromacs TPR format (Gromacs 4.0 to 4.6.1), see [Issue 2](https://code.google.com/p/mdanalysis/issues/detail?id=2))
  * fast XTC seeking ([Issue 127](https://code.google.com/p/mdanalysis/issues/detail?id=127))
  * changing resid (set\_resid()) or segid (set\_segid()) changes the topology and lists of resids/segids can be assigned to groups of objects (AtomGroup, ResidueGroup)
  * helanal: additional output of local bend and unit twist angles    ([Issue 133](https://code.google.com/p/mdanalysis/issues/detail?id=133))
  * added support for reading DMS files (DESRES molecular structure)
  * bond connectivity information can be guessed from a PDB file if    the bond=True keyword is set in Universe ([Issue 23](https://code.google.com/p/mdanalysis/issues/detail?id=23))
  * MDAnalysis.analysis.rms.RMSD: calculation of additional RMSDs
  * Plugin to generate nucleic acid helicoidal parameters using X3DNA;    (must install working version 2.1 of X3DNA independently)
  * can use advanced slicing (with arbitrary lists or arrays) at all    levels of the hierarchy ([Issue 148](https://code.google.com/p/mdanalysis/issues/detail?id=148))
  * coordinate readers and writers can be used as context managers    with the 'with' statement
  * Can load multiple trajectories as Universe(topology, traj2, traj2,    ...) in addition to providing all trajectories as a list,    i.e. Universe(topology, [traj1, traj2, ...])
  * added support for YASP and IBIsCO formats (.trz) ([Issue 152](https://code.google.com/p/mdanalysis/issues/detail?id=152))
  * new methods for AtomGroup: packIntoBox([inplace=True])
  * added non-standard "extended" PDB format (XPDB) that reads    five-digit residue numbers
  * util.convert\_aa\_code() recognizes non-standard residue names such    as HSE, GLUH, GLH, ...
  * added new geometrics selections: sphlayer, sphzone, cylayer, cyzone
  * added TopologyDict and TopologyGroup classes for bond analysis
  * added calc\_bonds, calc\_angles and calc\_torsions cython functions to    core.distances for quickly calculating bond information
  * added applyPBC(coords, box) function to core.distances to move     coordinates to within the primary unit cell
  * many AtomGroup methods now support 'pbc' flag to move atoms to within    primary unitcell before calculation.  This behaviour can also be     toggled using the core.flags['use\_pbc'] flag ([Issue 156](https://code.google.com/p/mdanalysis/issues/detail?id=156))
  * MDAnalysis.analysis.rms.rmsd(): new center keyword so that one can    immediately calculate the minimum rmsd of two rigid-body superimposed    structures

### Changes ###

  * libxdrfile2 is now used instead of libxdrfile. libxdrfile2 is distributed    under GPLv2
  * dropped support for Python 2.5; minimum requirement is Python 2.6    ([Issue 130](https://code.google.com/p/mdanalysis/issues/detail?id=130))
  * almost all methods of AtomGroup return NumPy arrays
  * slicing and indexing of AtomGroup, Residue, ResidueGroup, Segment, SegmentGroup will now always return an appropriate object and    never a simple list
  * removed Timeseries.principleAxis (probably was never working)
  * dependent on Biopython >= 1.59 ([Issue 147](https://code.google.com/p/mdanalysis/issues/detail?id=147))
  * Hydrogen bond analysis defaults to updating selection 1 and 2 for    every timestep in order to avoid unexpected behavior ([Issue 138](https://code.google.com/p/mdanalysis/issues/detail?id=138))
  * AtomGroup.velocities is now a (managed) attribute and not a method    anymore: replace 'ag.velocities()' with 'ag.velocities'
  * changed the name of the flag 'convert\_gromacs\_lengths' to 'convert\_lengths'

### Fixes ###

  * asUniverse now also accepts any instance that inherits from    MDAnalysis.Universe ([Issue 176](https://code.google.com/p/mdanalysis/issues/detail?id=176))
  * fixed XDR writer incorrect use of delta parameter ([Issue 154](https://code.google.com/p/mdanalysis/issues/detail?id=154))
  * fixed incorrect computation of distances in serial and parallel distance\_array() with PBC ([Issue 151](https://code.google.com/p/mdanalysis/issues/detail?id=151))
  * fixed [Issue 129](https://code.google.com/p/mdanalysis/issues/detail?id=129) (hole.py module pipe/file closure)
  * fixed array comparison bug in MDAnalysis.analysis.helanal and various enhancements to the helanal module
  * fixed MDAnalysis.analysis.rms.RMSD.run(): gave incorrect results if ref\_frame != 0
  * alignto() now checks that the two selections describe the same atoms (fixes [Issue 143](https://code.google.com/p/mdanalysis/issues/detail?id=143))
  * slicing of ResidueGroup will now produce a ResidueGroup, and slicing of a SegmentGroup will produce a SegmentGroup, not a list as before (fixes [Issue 135](https://code.google.com/p/mdanalysis/issues/detail?id=135))
  * detect OpenMP-capable compiler during setup ([Issue 145](https://code.google.com/p/mdanalysis/issues/detail?id=145)), which should allow users of Mac OS X 10.7 and 10.8 to build MDAnalysis using Apple's C-compiler (clang) ([Issue 142](https://code.google.com/p/mdanalysis/issues/detail?id=142)) although they will not get a parallel version of distance\_array.
  * PDB with blank lines gave IndexError ([Issue 158](https://code.google.com/p/mdanalysis/issues/detail?id=158))
  * fixed AtomGroup.ts Timestep instance not containing all available information ([Issue 163](https://code.google.com/p/mdanalysis/issues/detail?id=163))
  * fixed Timestep copy method returning a base Timestep rather than appropriate format ([Issue 164](https://code.google.com/p/mdanalysis/issues/detail?id=164))


## Authors ##
orbeckst, jandom, zhuyi.xue, xdeupi, tyler.je.reddy,         manuel.nuno.melo, danny.parton, sebastien.buchoux, denniej0,	  rmcgibbo, richardjgowers, lennardvanderfeltz, bernardin.alejandro,     matthieu.chavent