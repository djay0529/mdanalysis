This is a major new release of MDAnalysis, which contains a large number of enhancement and important [bug fixes](#Fixes.md) (especially related to treating unit cells in trajectories) and the number of UnitTests almost doubled to 1300. A small number of incompatible changes were introduced (see [Changes](#Changes.md) below) but they are not likely to affect user code. Major enhancements are a reworked system to deal with topological information, automatic use of offset files for Gromacs trajectories (to implement quick random access on XTC and TRR), support for more formats, support for stream IO instead of real files on disk (for some readers/writers) and more (see [Enhancements](#Enhancements.md))

From 0.9.0 onward, we will release more frequently and use SemanticVersioning to label the releases. The major impact on users will be that important bug fixes are rolled out quickly in "patch" releases (the next one will be 0.9.1).

## CHANGELOG ##

  * 0.9.0
  * released [17 March 2015](http://en.wikipedia.org/wiki/Saint_Patrick%27s_Day)


### Enhancements ###

  * offsets for XTC and TRR trajectories now stored and retrieved    automatically; improves init times for large trajectories ([Issue 208](https://code.google.com/p/mdanalysis/issues/detail?id=208))
  * docs now use secure mathjax CDN ([Issue 182](https://code.google.com/p/mdanalysis/issues/detail?id=182))
  * minor improvements to helanal docstring
  * Support for reading altloc records in PDB files
  * Cap proteins with ACE and NMA terminal caps
  * MOL2 read and write support
  * 2D streamplot code no longer uses deprecated matplotlib.nxutils module
  * core.distances.calc\_angles and calc\_torsions now accept an optional box    argument to consider periodic boundary conditions in their calculation ([Issue 172](https://code.google.com/p/mdanalysis/issues/detail?id=172))
  * TopologyGroup angles and torsions methods both have a pbc flag, (default    False) to toggle consideration of periodic boundaries
  * XYZWriter (write simple XYZ trajectories)
  * TRZReader upgrades, seeking and numframes faster
  * PQRWriter (write PQR files)
  * HydrogenBond analysis: new keyword distance\_type to alternatively    look at the distance between heavy atoms ([Issue 185](https://code.google.com/p/mdanalysis/issues/detail?id=185))
  * TopologyGroup/TopologyDict system overhauled. ([Issue 194](https://code.google.com/p/mdanalysis/issues/detail?id=194))
  * TopologyObject class created, Bonds/Angles/etc nicer to work with.
  * Topology information is now loaded lazily into Universe, can be forced to     load all with u.load\_topology(). All topology information is now stored in    .bonds .angles .torsions and .impropers attributes.
  * Added support for improper torsions.
  * AtomGroup now has .bonds .angles .torsions and .impropers attributes which    retrieve relevant TopologyGroups
  * Increased performance of topology.core.guess\_bonds greatly
  * Added topology.core.guess\_angles guess\_torsions and guess\_improper\_torsions    which given accurate bond information can calculate the rest of the    topology info.
  * Universe topology information is now settable after initialisation using lists of indices    such as those provided by the `guess_*` functions.
  * Added LAMMPS data parser for topology files with the .data suffix. Can also    read single frame coordinate & velocity information from these files.    ([Issue 139](https://code.google.com/p/mdanalysis/issues/detail?id=139))
  * Added Fragments.  Fragments are continuously bonded groups of atoms.    These are lazily built, and accessible via the Atom.fragment and AtomGroup.fragments    attributes.    ([Issue 190](https://code.google.com/p/mdanalysis/issues/detail?id=190))
  * Added ability to remove items from Universe cache with `_clear_caches`.
  * Added ability to define dimensions from AtomGroup, Universe and Timestep for most    formats.    ([Issue 203](https://code.google.com/p/mdanalysis/issues/detail?id=203))
  * streamIO: many readers can directly use gzip- or bzip2 compressed    files or a stream (such as cStringIO.StringIO) wrapped in    util.NamedStream; currently supported: PDB, PSF, CRD, PQR, PDBQT,    GRO, MOL2, XYZ
  * Added hydrogen bonding time autocorrelation analysis module    (analysis.hbonds.HydrogenBondAutoCorrel)
  * Added energy units to core.units ([Issue 214](https://code.google.com/p/mdanalysis/issues/detail?id=214))
  * New AtomGroup.split() method to produce a list of AtomGroups for each atom,    residue, or segment
  * New AtomGroup.sequence() method to extract a protein sequence.
  * Can pass subclasses of Reader and Topology reader to Universe init to allow    custom readers to be defined. ([Issue 198](https://code.google.com/p/mdanalysis/issues/detail?id=198))
  * Added Atom.bonded\_atoms property.  This returns an AtomGroup of the Atoms    that are bonded to a given Atom. ([Issue 219](https://code.google.com/p/mdanalysis/issues/detail?id=219))
  * Added atom selections to ContactAnalysis ([Issue 169](https://code.google.com/p/mdanalysis/issues/detail?id=169))


### Changes ###

  * DCD unitcell format changed: MDAnalysis will now read it as [A,    gamma, B, beta, alpha, C] instead of [A, alpha, B, beta, gamma,    C]. The new CHARMM box vector unitcell format is heuristically guessed.    (see [Issue 187](https://code.google.com/p/mdanalysis/issues/detail?id=187) for a full discussion and implications).
  * getstate() and setstate() raise an NotImplementedError for    Universe and AtomGroup; before they were silently accepted on    pickling and a cryptic "TypeError: AtomGroup is not callable" was    raised (see also [Issue 173](https://code.google.com/p/mdanalysis/issues/detail?id=173) for detailed explanation)
  * XTC/TRR reader raise IOError with errorcode EIO (instead of    ENODATA) when the last frame is reached and EBADF (instead of    EFAULT) for any other issues (partially addresses [Issue 150](https://code.google.com/p/mdanalysis/issues/detail?id=150),    Windows compatibility)
  * Universe.bonds now returns a TopologyGroup not a list.  TopologyGroup can be    iterated over; list(universe.bonds) should provide a fix to legacy code.
  * PQR reader will now set segid to a chainID if found in the PQR    file (previously, the segid would always be set to 'SYSTEM').
  * util.anyopen() only returns the stream and not the tuple (stream,    filename) anymore; it tries to set stream.name instead
  * topology reading now done via classes (similar to trajectory reading)    rather than functions.    ([Issue 210](https://code.google.com/p/mdanalysis/issues/detail?id=210))


### Fixes ###

  * fixed DCD triclinic unit cell reading and writing (although the new CHARMM    format with the box matrix is not supported for writing) ([Issue 187](https://code.google.com/p/mdanalysis/issues/detail?id=187))    '''ATTENTION: Support for triclinic boxes from DCDs was BROKEN prior to this fix!'''
  * fixed creation of residues and segments in Merge()
  * resolves [Issue 188](https://code.google.com/p/mdanalysis/issues/detail?id=188) regarding Helanal Finish Argument
  * fixed [Issue 184](https://code.google.com/p/mdanalysis/issues/detail?id=184) (TPR files with double precision)
  * fixed [Issue 199](https://code.google.com/p/mdanalysis/issues/detail?id=199) (FutureWarning in pyqcprot)

## Authors ##
richardjgowers, tyler.je.reddy, orbeckst, e.jjordan12, zhuyi.xue,     bala.biophysics, dotsdl, sebastien.buchoux