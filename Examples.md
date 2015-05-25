Add your own examples either to the page itself or in the comments at the bottom of the page.

**Contents**


## Frame-based analysis ##

A typical usage pattern is to iterate through a trajectory and analyze coordinates for every frame. In the following example the end-to-end distance of a protein and the radius of gyration of the backbone atoms is calculated:
```
import MDAnalysis
from MDAnalysis.tests.datafiles import PSF,DCD   # test trajectory
import numpy.linalg
u = MDAnalysis.Universe(PSF,DCD)                 # always start with a Universe
nterm = u.s4AKE.N[0]   # can access structure via segid (s4AKE) and atom name
cterm = u.s4AKE.C[-1]  # ... takes the last atom named 'C'
bb = u.selectAtoms('protein and backbone')  # a selection (a AtomGroup)
for ts in u.trajectory:     # iterate through all frames
  r = cterm.pos - nterm.pos # end-to-end vector from atom positions
  d = numpy.linalg.norm(r)  # end-to-end distance
  rgyr = bb.radiusOfGyration()  # method of a AtomGroup; updates with each frame
  print "frame = %d: d = %f Angstroem, Rgyr = %f Angstroem" % (ts.frame, d, rgyr)
```

## Molecular editing ##

MDAnalysis also has rudimentary abilities for structure editing and extraction.

### Extracting a chain from a PDB ###

Load a PDB, select what you want, and then write out the selection in PDB (or any other) [file format](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/init.html#supported-coordinate-formats):

```
import MDAnalysis
u = MDAnalysis.Universe('1ES7.pdb', permissive=False)    # permissive=False uses Bio.PDB (True will probably also work)
A = u.selectAtoms('segid A')
A.write('1ES7_A.pdb')
```

Note:
  * MDAnalysis uses PDB chain identifiers to set the segment id ("segid") of Segments. Thus for chain selections (which is PDB specific) you need to select for ''segid''.
  * When reading PDB files from the [Protein Data Bank](http://www.pdb.org/pdb/home/home.do) you can use the "strict" PDB reader (based on [biopython](http://biopython.org)'s Bio.PDB) via the ''permissive=False'' keyword. It will deal with insertion codes and other specialities of the [PDB file format](http://www.wwpdb.org/docs.html). If you know that the input structure is in a simple PDB format (or in any of the other [supported file formats](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/init.html#supported-coordinate-formats) then you can simply omit the ''permissive'' keyword.


## Interactive analysis of structures ##
### Residue charges in a PQR file ###
PQR files contain the atomic partial charges. If you want to know the charge on each residue (e.g. in order to check the actual protonation state assigned by [pdb2pqr](http://www.poissonboltzmann.org/pdb2pqr)) then you can do this quickly (e.g. in the ipython shell):
```
u = MDAnalysis.Universe("protein.pqr")
for r in u.residues:
  print("%s %d     %+4.2f" % (r.name, r.id, r.totalCharge())
```
This will print the residue and the total charge for all residues.

You can also define a function that returns the data in a list, prints it to screen or alternatively to a file _filename_:
```
def rescharges(u, filename=None, epsilon=0.01):
    if filename is not None:
        out = open(filename, "w")
    else:
        import sys
	out = sys.stdout
    resq = []
    for r in u.residues:
        q = r.totalCharge()
        record = (r.name, r.id, q)
	resq.append(record)
        out.write("%s %d     %+4.2f\n" % record)
    if filename is not None:
        out.close()
    return resq
```

## File format conversion ##

Converting a **single frame** is straightforward: read it into a universe and write out the atoms. For instance, converting from PQR to PDB:
```
from MDAnalysis import Universe
u = Universe("system.pqr")
u.atoms.write("system.pdb")
```

In order to **convert trajectories** you obtain a trajectory Writer for the desired output, loop through the input trajectory frame by frame, and write each frame to the output (see [dcd2xtc.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/dcd2xtc.py) for the conversion from DCD to XTC. The core of this short script is:
```
from MDAnalysis import Universe, Writer
u = Universe("system.psf", "system.dcd")
w = Writer("system.xtc", u.trajectory.numatoms)
for ts in u.trajectory:
    w.write(ts)
w.close_trajectory()
```
(You might also want to write a PDB file to be used together with the XTC.)


## Simple scripts ##

Have a look at the [examples](http://code.google.com/p/mdanalysis/source/browse/#git%2Fpackage%2Fexamples) subdirectory for some other examples.
Note: These examples were all written using numpy version 1.0.2. Older versions of numpy may not work correctly (since the api wasn't finalized until 1.0). Some of the scripts are incomplete in that one has to enter a psf and a dcd file somewhere at the top. The main purpose is to give an idea of how to quickly code up some interesting analysis tasks with the help of MDAnalysis.

  * Rotational autocorrelation ([rotational\_autocorrelation.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/rotational_autocorrelation.py))
  * Lipid order parameters ([lipid\_order\_parameters.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/lipid_order_parameters.py))
  * Potential profile across double bilayer system ([potential\_profile.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/potential_profile.py))
  * Radial distribution function of water in a pure water box ([radial\_distribution\_function.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/radial_distribution_function.py))
  * Schlitter entropy calculated based using the determinant ([schlitter\_determ.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/schlitter_determ.py)) or by calculating the eigenvalues ([schlitter\_quasiharmonic.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/schlitter_quasiharmonic.py)) of the covariance matrix.

## Analysis module ##

Since MDAnalysis 0.6.2 there exists a collection of analysis modules. One can use them by importing the appropriate module
```
import MDAnalysis.analysis
help(MDAnalysis.analysis)           # see what's available
import MDAnalysis.analysis.contacts # use the native-contacts analysis
```
The [source code of the analysis sub-modules](http://code.google.com/p/mdanalysis/source/browse/#git%2Fpackage%2FMDAnalysis%2Fanalysis) can be used to learn how to do fairly complicated things with MDAnalysis.

Note that some of the sub-modules require additional python packages. One can automatically install dependencies for the _analysis_ optional package with (something like... see [InstallRecipes](InstallRecipes.md))
```
  cd MDAnalysis-0.6.3
  easy_install-2.6 . MDAnalysis[analysis] 
```
(The optional package is added in brackets after the name of the package; see the easy\_install docs.)

## Parallel analysis on multiple cores ##
  * A limited number of functions have parallel versions such as the distance computations in [MDAnalysis.core.parallel.distances](http://pythonhosted.org/MDAnalysis/documentation_pages/core/parallel.html). When they are used instead of the serial code they use all available cores.
  * As demonstrated in [Multicore\_MDAnalysis](Multicore_MDAnalysis.md) one can also use the Python [multiprocessing](http://docs.python.org/2/library/multiprocessing.html) module to split analysis tasks and work on them in parallel.