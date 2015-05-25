# Changes #

  * 0.7.3 release

## Removals ##
  * completely removed the old `core.rms_fitting` module (and thus we also do not depend on the LAPACK library anymore, which should simplify installation); use the functions accessible through [MDAnalysis.analysis.align](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/align.html?highlight=mdanalysis.analysis.align#MDAnalysis.analysis.align) (which are faster and use QCPROT)

## Enhancements ##

  * [PDBQT](http://autodock.scripps.edu/faqs-help/faq/what-is-the-format-of-a-pdbqt-file) (AutoDOCK) format added (reading and writing of single frames)
  * new attributes [universe.trajectory.frame](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/base.html?highlight=reader.frame#MDAnalysis.coordinates.base.Reader.frame) and [universe.trajectory.time](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/coordinates/base.html?MDAnalysis.coordinates.base.Reader.time#MDAnalysis.coordinates.base.Reader.time): report the current frame number and time (e.g. in ps) of the current frame of the trajectory
  * new attribute [Timestep.volume](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/coordinates/base.html?MDAnalysis.coordinates.base.Reader.time#MDAnalysis.coordinates.base.Timestep.volume) (unit cell volume)
  * **HELANAL helix analysis** in [MDAnalysis.analysis.helanal](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/helanal.html); Python implementation of [helanal.f](http://www.ccrnp.ncifcrf.gov/users/kumarsan/HELANAL/helanal.f) from http://www.ccrnp.ncifcrf.gov/users/kumarsan/HELANAL/helanal.html (Benjamin Hall, used under GPL v2+)
  * hydrogen bonds analysis in [MDAnalysis.analysis.hbonds](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/hbonds.html)
  * [MDAnalysis.analysis.distances.dist()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/distances.html?highlight=mdanalysis.analysis.distances.dist#MDAnalysis.analysis.distances.dist) for calculating distances between matching atoms in two groups
  * MDAnalysis logo by Christian Beckstein from the LogoCompetition (and reformatting of the online docs to match the logo theme)


## Change of behaviour ##

  * [alignto()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/align.html?highlight=alignto#MDAnalysis.analysis.align.alignto) and [rms\_fit\_trj()](http://mdanalysis.googlecode.com/svn/trunk/doc/html/documentation_pages/analysis/align.html?highlight=rms_fit_trj#MDAnalysis.analysis.align.rms_fit_trj): changed keyword 'select' default from 'backbone' to 'all'

## Fixes ##

  * fixed [alignto()](http://mdanalysis.googlecode.com/git/doc/html/documentation_pages/analysis/align.html?highlight=alignto#MDAnalysis.analysis.align.alignto) (raised KeyError)
  * fixed [Issue 57](https://code.google.com/p/mdanalysis/issues/detail?id=57) (check for illegal coordinates when writing PDB and GRO)
  * use spaces everywhere and no TABs and tell emacs and vim to keep it that way ([Issue 69](https://code.google.com/p/mdanalysis/issues/detail?id=69))
  * fixed [Issue 70](https://code.google.com/p/mdanalysis/issues/detail?id=70) (problems with instant atom selections)

# Authors #
orbeckst, jandom, Benjamin Hall, Paul Rigor,  dcaplan, Christian Beckstein (logo), denniej0