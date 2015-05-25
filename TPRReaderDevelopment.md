# Introduction #

Gromacs combines structure and topology information into a binary tpr file. This file is used as a run file and is not user-readable; it can be printed out using 'gmxdump' command.

The aim is to be able to read a tpr just like any other structure/topology file format. No write support is intended at this stage.

# Current implementation in MDAnalysis #
The implementation has been discussed in [Issue 2](https://code.google.com/p/mdanalysis/issues/detail?id=2).

[alfred532008's TPR reader](https://code.google.com/r/alfred532008-mdanalysis-tprreader/source/list?name=tprparser) is available as the [feature branch pyTPRparser](https://code.google.com/p/mdanalysis/source/browse/package?name=pyTPRparser) in the repository.

Anyone who wants to play with the code should be able to do so with commands such as
```
git fetch origin
git checkout pyTPRparser
```
Current limitations:
  * limited TPR file versions (as of March 2013):
    * version 58 (Gromacs releases 4.0.x)
    * version 73 (Gromacs releases 4.5.x)
  * does not read all parts of the TPR file (see source code of [TPRparser.py](https://code.google.com/p/mdanalysis/source/browse/package/MDAnalysis/topology/TPRParser.py?name=pyTPRparser))

For additional information see the developer thread [mdnalysis-devel: Issue 2 in mdanalysis: reading Gromacs tpr files instead of psf](https://groups.google.com/d/topic/mdnalysis-devel/nMwUjAZR-iQ/discussion).


# Developer notes #
## Details ##

The relevant GROMACS source files are
- src/programs/gmxdump
- gromacs/gmxlib/tpxio.c
- gromacs/gmxlib/gmxfio.c

The binary file can be read-in in python using xdrlib http://docs.python.org/py3k/library/xdrlib.html

## Conventions ##

It would help not to simplify/expand gromacs variable names and data structure. It's hard enough getting ones had around what's going on in gromacs source right now. It will only introduce confusion to have two naming conventions in the future, when the gromacs devs introduce changes to the tpr format.

## Example TPR reader ##
```

__author__      = "Jan Domanski"
__copyright__   = "GNU Public Licence, v2"

import xdrlib

def main():
  """
  Based on the following gromacs source files
    - gmxdump.c
    - tpxio.c (the most important one)
    - gmxfio.h
  """

  tpr = "adk_oplsaa.tpr"

  f = open(tpr).read() 

  data = xdrlib.Unpacker(f) 

  # From src/gromacs/legacyheaders/typedefs.h
  STRLEN = 4096
  BIG_STRLEN = 1048576

  number = data.unpack_int()

  version_string = data.unpack_string()

  precision = data.unpack_int()

  fver = data.unpack_int()

  gen = data.unpack_int()

  tpx_natoms = data.unpack_int()

  return

  if fver >= 28:
    tpx_ngtc = data.unpack_int()
  else:
    tpx_ngtc = 0 
    
  if fver < 62:
    idum = data.unpack_int()
    rdum = data.unpack_float()
    
  tpx_lambda0 = data.unpack_float()
  tpx_bIr =  data.unpack_int()
  tpx_bTop =  data.unpack_int()
  tpx_bX =  data.unpack_int()
  tpx_bV =  data.unpack_int()
  tpx_bF =  data.unpack_int()  
  tpx_bBox =  data.unpack_int()    
  
if __name__ == "__main__":
  main()
```