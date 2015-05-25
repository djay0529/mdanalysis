The paper on MDAnalysis has been published as

  * N. Michaud-Agrawal, E. J. Denning, T. B. Woolf, and O. Beckstein. MDAnalysis: A Toolkit for the Analysis of Molecular Dynamics Simulations. _J. Comput. Chem._ **32** (2011), 2319–2327, DOI: [10.1002/jcc.21787](http://dx.doi.org/10.1002/jcc.21787)

The [HTML version of the paper](http://onlinelibrary.wiley.com/doi/10.1002/jcc.21787/full) has so many indentation errors in the code example to make it almost completely useless for learning MDAnalysis. The [PDF](http://onlinelibrary.wiley.com/doi/10.1002/jcc.21787/pdf) version is mostly correct although the final formatted version ended up with a few line breaks that are illegal in the Python syntax. The free PubmedCentral version (available April 2012, e.g. through [PMID 21500218](http://www.ncbi.nlm.nih.gov/pubmed/21500218)), however, contains perfectly formatted Python code.

Corrected code is listed here and can also be found in the examples that are part of the MDAnalysis package. We are sorry for the inconvenience but assure you that we tried our best to impart the importance of spaces in Python code on the typesetter.

We are also collecting any further errata (which correct errors or omissions in both the PDF and the HTML version). If you find further problems then please let us know by emailing one of the authors or filing an issue in the MDAnalysis [Issue Tracker](http://code.google.com/p/mdanalysis/issues/list).

## Errata ##
### page 4 ###
#### Writing the coordinates of a selection ####
```
universe.selectAtoms("byres (resname SOL and around 3.5 protein)").write("solvation-shell.pdb")
```
### page 5 ###
#### Block average example ####
The **PDF** version also contains a crucial indentation error.
```
def blocked(universe, nblocks, analyze): 
    size = universe.trajectory.numframes/nblocks
    blocks = [] 
    for block in xrange(nblocks): 
        a = [] 
        for ts in u.trajectory[block*size:(block+1)*size]: 
            a.append(analyze(universe)) 
        blocks.append(numpy.average(a)) 
    blockaverage = numpy.average(blocks) 
    blockstd = numpy.std(blocks) 
    return nblocks, size, blockaverage, blockstd
```
See [examples/blocks.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/blocks.py) for a working example.

#### Radius of gyration analysis function ####
```
def rgyr(universe): 
    return universe.selectAtoms("protein").radiusOfGyration()
```

#### Time series analysis ####
```
MDAnalysis.collection.addTimeseries(\ 
    Timeseries.Dihedral(atomselection)) 
data = universe.trajectory.correl(\
    MDAnalysis.collection, skip=10)
```

### page 6 ###
#### radial distribution function ####
The normalisation is not correct in the simplified example in the paper; use [examples/radial\_distribution\_function.py](http://code.google.com/p/mdanalysis/source/browse/package/examples/radial_distribution_function.py) from the distribution.

### Table 1 ###
[CHARMM](http://www.charmm.org) has a way to calculate 3D densities using the [COORdinates ANALysis ... IHIS](http://www.charmm.org/html/documentation/c35b1/corman.html) facility. The table states incorrectly that CHARMM can not presently carry out this kind of calculations.