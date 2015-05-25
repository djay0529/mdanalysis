# Availability #

Either **download the tar file** [MDAnalysis-0.6.2-rc1.tar.gz](http://code.google.com/p/mdanalysis/downloads/detail?name=MDAnalysis-0.6.2-rc1.tar.gz) from http://code.google.com/p/mdanalysis or **checkout the release-0-6-2 branch**
```
svn checkout http://mdanalysis.googlecode.com/svn/branches/release-0-6-2 mdanalysis-0.6.2
```
or use **setuptools**' [easy\_install](http://peak.telecommunity.com/DevCenter/EasyInstall) directly (_new!_)
```
easy_install -f http://code.google.com/p/mdanalysis MDAnalysis
```

Any feedback through the [Issue Tracker](http://code.google.com/p/mdanalysis/issues/list) or the [Mailing List](http://groups.google.com/group/mdnalysis-discussion) is very much appreciated.


# Major changes #

Automatic installations with [setuptools](http://trac.edgewall.org/wiki/setuptools) should be working now (_if the LAPACK library is in a canonical place such as /usr/lib_), i.e. you can simply install the latest version of _MDAnalysis_ with
```
easy_install -f http://code.google.com/p/mdanalysis/downloads/list MDAnalysis
```
or upgrade with
```
easy_install -f http://code.google.com/p/mdanalysis/downloads/list --upgrade MDAnalysis
```
If this does not work, unpack the source as usual and customize `setup.cfg` as described in [Install](Install.md). Then run `easy_install` on the sources
```
easy_install ./MDAnalysis-0.6.2 MDAnalysis[tests]
```

If you want to install additional required packages for the UnitTests then list the _tests_ requirement:
```
easy_install -f http://code.google.com/p/mdanalysis/downloads/list MDAnalysis[tests]
```

# Changes #

  * 0.6.2 release
  * removed a number of imports from the top level (such as rms\_fitting); this might break some scripts that still rely on the layout that was used for 0.5.x (which is now officially declared deprecated)
  * defined trajectory API
  * deprecated `DCD.dcd_header` was renamed to `DCD._dcd_header`
  * XTC and TRR compute numframes by iterating through trajectory (slow!)
  * introduced units: base units are ps (time) and Angstrom (length); see [core.flags](http://code.google.com/p/mdanalysis/source/browse/branches/release-0-6-2/MDAnalysis/core/__init__.py#199)
  * XTC and TRR automatically convert between native Gromacs units (ps, nm) and base units (uses `core.flags['convert_gromacs_lengths'] = True`)
  * more test cases (see UnitTests)
  * _really_ FIXED [Issue 16](https://code.google.com/p/mdanalysis/issues/detail?id=16) (can `easy_install` from tar file)
  * FIXED a bug in AtomGroup.principalAxes() (in [r297](https://code.google.com/p/mdanalysis/source/detail?r=297))
  * added dependency information to [setup.py](http://code.google.com/p/mdanalysis/source/browse/branches/release-0-6-2/setup.py) ([numpy](http://numpy.scipy.org) by default; [nose](http://somethingaboutorange.com/mrl/projects/nose) for tests)