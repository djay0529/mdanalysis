# Description #

The [GridDataFormats package](http://pypi.python.org/pypi/GridDataFormats) provides classes to unify reading and writing of n-dimensional datasets. One can read grid data from files, make them available as a `Grid` object, and allows one to write out the data again.

The package provides the module **gridData**, which is loaded with
```
import gridData
```

**gridData** is used in MDAnalysis in the [MDAnalysis.analysis.density](http://mdanalysis.googlecode.com/git/package/doc/html/documentation_pages/analysis/density.html) module.


# Availability #

The **GridDataFormats package** is available from http://pypi.python.org/pypi/GridDataFormats and can simply be installed with
```
easy_install GridDataFormats
```
or
```
pip install GridDataFormats
```

The package can make use of SciPy, so this should be installed beforehand.

The source code and latest development are available from https://github.com/orbeckst/GridDataFormats