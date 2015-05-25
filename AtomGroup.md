The [AtomGroup](http://code.google.com/p/mdanalysis/source/browse/MDAnalysis/core/AtomGroup.py#108) class is the most important object in MDAnalysis. It contains a list of [Atom](http://code.google.com/p/mdanalysis/source/browse/MDAnalysis/core/AtomGroup.py#38)s and is described in under [Fundamental building blocks](http://mdanalysis.googlecode.com/git-history/develop/package/doc/html/documentation_pages/core/AtomGroup.html) in the [online documentation](http://mdanalysis.googlecode.com/git-history/develop/package/doc/html/index.html).

## Creating an AtomGroup ##

The atoms in a [Universe](Universe.md) are a AtomGroup
```
>>> import MDAnalysis
>>> from MDAnalysis.tests.datafiles import *
>>> u = MDAnalysis.Universe(PSF,DCD)
>>> type(u.atoms)
<class 'MDAnalysis.core.AtomGroup.AtomGroup'>
```

Any [selection](Documentation#Selections.md) produces a AtomGroup
```
>>> calphas = u.selectAtoms("name CA")
>>> type(calphas)
<class 'MDAnalysis.core.AtomGroup.AtomGroup'>
```


Multiple AtomGroup instances can be joined using addition:
```
>>> arginines = u.selectAtoms('resname ARG')
>>> lysines = u.selectAtoms('resname LYS')
>>> basics = arginines + lysines
```
(Of course, this example is contrived and one could have simply used `basics = u.selectAtoms('resname ARG or resname LYS')`.)

A AtomGroup can be manually created by supplying a list of [Atom](Atom.md) instances to the class constructor
```
>>> manual_atom_selection = [u.atoms[5], u.atoms[111], u.atoms[1000]] + u.atoms[-10:]
>>> manual_atomgroup = MDAnalysis.core.AtomGroup.AtomGroup(manual_atom_selection)
>>> print manual_atomgroup
<AtomGroup with 13 atoms>
```

but this is rarely necessary; typically one simply uses a selection.

## Using an AtomGroup ##

The AtomGroup instance has a number of methods that compute properties over all atoms in the group. The _values_ that these methods return _can change_ when one steps through the trajectory on which the selection is based.

The list of [Atom](Atom.md) instances can be accessed as
<dl>
<dt>a.atoms</dt>
<dd>list of <a href='Atom.md'>Atom</a> objects</dd>
</dl>


### Partial list of useful AtomGroup methods ###
For a atom group `a`:
<dl>
<dt>a.coordinates()</dt>
<dd>numpy array of all coordinates</dd>
<dt>a.centerOfGeometry()</dt>
<dd>mean position of all atoms</dd>
<dt>a.centerOfMass()</dt>
<dd>center of mass of the atoms; needs masses to be defined</dd>
<dt>a.principalAxes()</dt>
<dd>the three principal axes of the collection of atoms; needs the masses in order to calculate the moments of inertia. The eigenvectors are sorted by eigenvalue, with the first one corresponding to the highest eigenvalue.</dd>
<dt>a.numberOfAtoms()</dt>
<dd>number of atoms</dd>
<dt>a.numberOfResidues()</dt>
<dd>number of residues that include those atoms</dd>
<dt>a.radiusOfGyration()</dt>
<dd>radius of gyration</dd>
<dt>a.totalCharge()</dt>
<dd>sum of all partial charges (only useful when the topology contained charges)</dd>
<dt>a.totalMass()</dt>
<dd>sum of all masses</dd>
</dl>

One can also make a **sub-selection** using
<dl>
<dt>a.selectAtoms(<i>selection-string</i>)</dt>
<dd>standard selection with the limitation that one can only access atoms that are part of the atom group (see <a href='https://code.google.com/p/mdanalysis/issues/detail?id=10'>Issue 10</a> and <a href='https://code.google.com/p/mdanalysis/issues/detail?id=42'>Issue 42</a>).</dd>
</dl>



For more information the documentation string that can be obtained with
```
>>> help(MDAnalysis.core.AtomGroup.AtomGroup)
```