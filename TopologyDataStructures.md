MDAnalysis primarily uses a list of Atoms to identify particles in the system. Topology information (i.e. connectivity through bonds and force-field relevant higher order connectivities such as angles (3) and torsions (dihedrals, 4)) is partially available. This depends on the topology parser (e.g. the PSFParser and TPRParser can read this information from the topology file).

Bond information can also be guessed from a distance search using topology.core.guess\_bonds().  Once bond information is in place, angles and torsions can be determined using topology.core.guess\_angles() and guess\_torsions().

This page together with the [Developer Notes on Topologies in the online docs](http://mdanalysis.googlecode.com/git-history/develop/package/doc/html/documentation_pages/topology/init.html#developer-notes) serves as a starting point for discussions and to spell out a number of implementation details that so far have not been explicitly defined.

**Contents**



# Overview #
Currently, the topology information is stored in an ad-hoc manner in the attribute **`Universe._psf`**. This is a dict with entries
  * `_atoms`
  * `_bonds`
  * `_angles`
  * `_dihe`
  * `_impr`

Only `_atoms` _must_ be present and it contains a list of AtomGroup.Atom instances.
All other entries must be a list of tuples containing the atom numbers of the bonds.
Universe has `.bonds` `.angles` `.torsions` and `.impropers` attributes which are loaded lazily into `__cache` the first time they are called. This can be forced using universe.build\_topology().
Building topology calls a relevant `universe._init*` method.  This method reads from the appropriate field in `u._psf`, and creates TopologyObjects.  On initialisation, TopologyObjects append themselves to Atom objects.


## Numbering ##
There are different numbering schemes for **Atom** instances in simultaneous use:
  * **Atom.number** uniquely identifies an Atom in MDAnalysis.
  * **Atom.serial** is typically atom.number+1 but really can be set by the parser.
  * **Atom.id** is the atom number _inside_ a residue, starting at 0. It is **not** unique.
The **Atom.number** is the **unique identifier** for an atom.


# Topology Group #

## Basics ##

`TopologyGroup` objects are designed to be a container for many TopologyObjects analogous to AtomGroups containing Atoms.
The Universe attributes detailed above are master versions of these TopologyGroups.
AtomGroups can then build their own ones lazily from their Atoms.

A single TopologyGroup can only contain a single type of Topology, eg only Bonds, not a mix of Bonds and Atoms.

## Topology Dict ##

TopologyGroups have a lazily created `.topDict` attribute which categorises TopologyObjects based on their `.type`, which is a tuple of `Atom.type`s.
TopologyGroups of bonds can then be selected based on types, eg:

```
tg = u.bonds  # all bonds in universe as object
tg.types()  # view available bonds
tg.selectBonds(('C', 'H'))  # select all C-H bonds in Universe.

ag.u.atoms[100:200]
tg2 = ag.angles.selectBonds(('C', 'C', 'O'))  # select all C-C-O atoms in an AtomGroup.
```

## Atomgroup intersection ##

TopologyGroups and AtomGroups can be intersected as if they were sets

```
ag = u.atoms[0:100]  # select some atoms
tg = u.bonds.selectBonds(('C', 'O'))  # select a type of bond
tg.atomgroup_intersection(ag)

# this is identical to ag.bonds.selectBonds(('C', 'O'))!
```

By default, bonds will be included in an AtomGroup if at least one Atom has a bond.
To only include bonds which have all their atoms within the AtomGroups, there is a keyword `strict`

```
ag = u.atoms[0:100]  # select some atoms
tg = u.bonds.selectBonds(('C', 'O'))  # select a type of bond
tg.atomgroup_intersection(ag, strict=True)
```

## Calculations ##

Cython functions have been written for bond length, angle size and torsion size including the use of periodic boundary conditions.  These are callable from TopologyGroups using the `bonds()`, `angles()` and `torsions()` methods.

# Topology Objects #

All bonds/angles/etc are a subclass of TopologyObject.
This is a container which stores Atom objects in a tuple in `.atoms`.
This then supports operations such as `__getitem__` `__contains__` `__eq__` operating on the tuple of atoms.

## Bonds ##
Bonds are 1-2 interactions between two atoms.

### Data structures ###
  1. `Universe._psf['_bonds']`
  1. `Universe.bonds`
  1. `AtomGroup.bonds`

## Angles ##
Angles are 1-2-3 interactions between three atoms. The atom 2 is at the apex of an angle.

### Data structures ###
  1. `Universe._psf['_angles']`
  1. `Universe.angles`
  1. `AtomGroup.angles`


## Dihedrals ##
Dihedrals or torsions are 1-2-3-4 interactions between four atoms. They describe the angle between the planes formed by atoms 1-2-3 and 2-3-4. (Proper and improper dihedrals are computed in the same manner; only the order of atoms differs.)

### Data structures ###

  1. `Universe._psf['_dihe']`
  1. `Universe.torsions`
  1. `AtomGroup.torsions`
  1. `Universe._psf['_impr']`
  1. `Universe.impropers`
  1. `AtomGroup.impropers`