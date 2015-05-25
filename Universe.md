The [Universe](http://mdanalysis.googlecode.com/git/package/doc/html/documentation_pages/core/AtomGroup.html#MDAnalysis.core.AtomGroup.Universe) class represents a whole simulation system. It is constructed from a **topology** (describing atom types and possibly connectivity) and a **trajectory** (the coordinates of the individual atoms, possibly as a function of time).

Almost every MDAnalysis script starts with
```
import MDAnalysis
u = MDAnalysis.Universe(topology, trajectory)
```

The object `u` encapsulates (almost) everything that MDAnalysis can do with a simulation system.