This package analyses molecular dynamics trajectories or conformational ensembles in terms of spin-label distances as probed in double electron-electron resonance (DEER) experiments. The spin labels are fitted on trajectories and the spin label mobility is taken into account using a rotamer library.

  * Author:    Philip W Fowler, Oliver Beckstein
  * Year:      2013
  * Licence:   GNU Public Licence, version 2 (or higher)
  * Copyright: © 2013 Philip W Fowler, Oliver Beckstein
  * Citation:  LS Stelzl, PW Fowler, MSP Sansom, O Beckstein. _J Mol Biol_ 426 (2014), 735–751 doi:10.1016/j.jmb.2013.10.024  (see Ref 3)
  * Source code: https://mdanalysis.googlecode.com
  * Packages: https://pypi.python.org/pypi/RotamerConvolveMD

This package contains the MTSL rotamer library R1A\_298K provided by Gunnar Jeschke, which is published under the GPL with his permission.



# Background #
Double electron electron spin resonance (DEER) is an EPR technique for measuring distances between two spin labels that have been covalently attached to a protein. Two cysteine residues are introduced into the protein and subsequently labelled. The positions are chosen to report on the expected conformational change. A commonly used spin label is (1-oxyl-2,2,5,5-tetramethylpyrroline-3-methyl)-methanethiosulfonate (MTSL). MTSL has a linker with five rotatable bonds and is therefore very flexible. The distance distributions between the two spin labels measured by experiments are typically broad and often multi-modal. The distributions are therefore a convolution of the flexibility of the MTSL spin label and the conformational spread of the proteins in the sample. To ensure that we compared like with like we developed a method that

  1. maps rotamer libraries of the MTSL spin label onto each position,
  1. discards those rotamers that sterically clash with the protein (typically distances <2 Å) and
  1. calculates all (weighted) distance pairs between the remaining rotamers and
  1. thereby estimates a distance distribution for that structure.

The code was written in Python using the MDAnalysis library<sup>1</sup> and a published rotamer library for MTSL<sup>2</sup>. It is available for download from the MDAnalysis website, http://mdanalysis.googlecode.com.

Our approach improves upon the existing method<sup>2</sup> by increasing computational efficiency and implementing, via the MDAnalysis library, analysis of ensembles of hundreds of structures, which allowed us to estimate distance distributions for entire simulation trajectories.

Examples of the application of this approach can be found in Ref 3.


# Installation #
## Release Packages ##
Python packages are hosted on PyPi in the [RotamerConvolveMD project](https://pypi.python.org/pypi/RotamerConvolveMD). You may download releases from there manually or simply have them downloaded automatically, e.g.
```
pip install RotamerConvolveMD
```
or
```
easy_install RotamerConvolveMD
```
This should also install MDAnalysis if needed.  If problems arise, try installing MDAnalysis first (see http://mdanalysis.googlecode.com for help).

## From source ##
Get the source (either as a tarball or a git checkout) and install the package from its own top directory with
```
python setup.py install --user
```
(see also the file INSTALL)

This will automatically install MDAnalysis and other dependencies. If problems arise, try installing MDAnalysis first (see http://mdanalysis.googlecode.com for help).

Analysis is performed with the script `convolve-mtss-rotamers.py`, which will have been installed in your `bin` directory. You might have to add the bin directory to your PATH. Consult your Python documentation for the details although often this will be `~/.local/bin` (for a typical `--user` installation or `/usr/local/bin` for site-wide installation).


# Usage #
Analysis is performed with the script `convolve-mtss-rotamers.py`. It takes as input

  * a topology or structure file (PSF, GRO, PDB, ... any topology format recognized by MDAnalysis)
  * a trajectory (DCD, XTC, TRR, ... any format that MDAnalysis can read)

A typical invocation:
```
convolve-mtss-rotamers.py --resid 47 330  --histogramBins 0 80 1  --clashDistance 2.2  \
       --output "dat/peptso-xrd"  --dcdfilename "dcd/peptso-xrd-47-330" \
       peptso.gro peptso.xtc 
```
It loads the MD trajectory from the topology `peptso.gro` and the trajectory `peptso.xtc`. The `--resid` pair is required and denotes the residue numbers (in the topology) to which the MTSSL spin labels would be attached. Rotamers that overlap with protein atoms as measured by an atom-atom distance smaller than the `--clashDistance` will be discarded and not counted in the distance calculations. For further explanations see the `--help` option.

For an example, see ```doc/example``` in the source distribution. The example can also be run to test the installation as reference output is provided.


# Help #
If you have questions or problems installing the package then ask on the [MDAnalysis user mailing list](http://groups.google.com/group/mdnalysis-discussion).


# References #
Please cite the following references when using this application:
  1. N. Michaud-Agrawal, E. J. Denning, T. B. Woolf, and O. Beckstein. MDAnalysis: A toolkit for the analysis of molecular dynamics simulations. _J Comp Chem_, **32**:2319-2327, 2011. doi:[10.1002/jcc.21787](http://doi.org/10.1002/jcc.21787). http://mdanalysis.googlecode.com
  1. Y. Polyhach, E. Bordignon, and G. Jeschke. Rotamer libraries of spin labelled cysteines for protein studies. _Phys. Chem. Chem. Phys._, **13**:2356-2366, 2011. doi:[10.1039/C0CP01865A](http://doi.org/10.1039/C0CP01865A)
  1. L. S. Stelz, P. W. Fowler, M. S. P. Sansom, and O. Beckstein. Flexible gates generate occluded intermediates in the transport cycle of LacY. _J Mol Biol_ **426**:735-751 2014 doi:[10.1016/j.jmb.2013.10.024 ](http://doi.org/10.1016/j.jmb.2013.10.024)