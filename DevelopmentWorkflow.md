**Contents**


# Development workflow #

As of 2012-02-21, the git repository structure was modified to reflect the development workflow described here (see [issue 96](https://code.google.com/p/mdanalysis/issues/detail?id=96) for discussion).

Please check [RepositoryStructure](RepositoryStructure.md) for details about the git repository structure.

## `master` branch vs `develop` branch ##
The code repo is composed of two main branches:
  * the `master` branch
  * the `develop` branch

The `master` branch is used to store _production-ready_ code. As a consequence, development code should _never_ be committed to this branch. Ideally, only the release manager should commit code to this branch when a release is ready (See below).

The `develop` branch is where the magic happens! This is the branch used for development code. To avoid breaking things periodically, only working (and tested!) code should be committed to this branch. In other words, please consider this branch as an "integration branch" used to include your code into the next release.

## Code integration into `develop` ##

As already mentioned, only "stabilized" code should be incorporated into the main development branch `develop`. When adding new features to MDAnalysis, developpers are asked to add the corresponding tests to the testsuite. Please check [UnitTests](UnitTests.md) for details.

## Supporting branches ##
In addition to the `master` and `develop` branches, Three kinds of supporting branches may be used:
  * `feature` branches
  * `release` branches
  * `hotfix` branches

### feature branches ###

When you want to add an more-or-less experimental feature but still want other developers to be able to contribute (i.e. don't want to do everything locally), you may use a `feature` branch.

Rules for a `feature` branch:
  * May branch from: `develop`
  * Must be merged into: `develop`
  * Naming convention: about anything except `master`, `develop`, `release-*`, `hotfix-*`

#### Typical workflow for feature development ####

  * Create the branch from the `develop` branch:
```
git checkout -b myfeature develop
```
  * Do some hacking... and commit
  * Publish your branch (required only if you want to ease collaborative work):
```
git push -u origin myfeature
```
  * More hacking and commits
  * When your feature is ready to be implemented into the next release, merge back to the `develop` branch and publish it:
```
git checkout develop
git merge --no-ff myfeature
git push origin develop
```

Note: the `--no-ff` is used to prevent history loss

  * Finally, you can delete your `feature` branch:
```
git branch -d myfeature
```
  * ... And don't forget to delete the corresponding published branch (if you published it):
```
git push origin :myfeature
```

### Release branches ###

`release` branches are used to prepare a new production release.
They should be handled by the release manager only.

Rules for a `release` branch:
  * May branch from: `develop`
  * Must be merged into: `master` (and `develop` if needed)
  * Naming convention: `release-*` where **should be a version number**

#### Typical workflow for preparating a release ####

  * Create the branch from the `develop` branch:
```
git checkout -b release-0.7.6 develop
```

  * Make sure the version number is right:
```
./maintainer/change_release.sh 0.7.6
```
  * Check that everything is ready (documentation, unittests...)
  * Commit the finalized state:
```
git commit -m "release 0.7.6 ready"
```
  * Merge the branch into `master` and tag the release:
```
git checkout master
git merge --no-ff release-0.7.6
git tag -a release-0.7.6
```

  * Merge the branch back into `develop` (actually this is not required if the only change was the version number):
```
git checkout develop
git merge --no-ff release-0.7.6
./maintainer/change_release.sh 0.7.7-devel
git commit -a -m "version number changed to 0.7.7-devel"
```
  * Delete the `release` branch:
```
git branch -d release-0.7.6
```

### the hotfix branches ###
`hotfix` branches are used to fix an issue found in a already released version.
Like the `release` branches, they should be handled by the release manager only.

Rules for a `feature` branch:
  * May branch from: `master`
  * Must be merged into: `master` (and `develop` if needed)
  * Naming convention: `hotfix-*` where **should be a version number**

#### Typical workflow for preparating a hotfix ####

  * Create the branch from the `master` branch:
```
git checkout -b hotfix-0.7.6.1 master
```

  * Make sure the version number is right:
```
./maintainer/change_release.sh 0.7.6.1
```
  * Fix what has to be fixed.
  * Commit the fixed state:
```
git commit -m "issue #123 fixed"
```
  * Merge the branch into `master` and tag the release:
```
git checkout master
git merge --no-ff hotfix-0.7.6.1
git tag -a hotfix-0.7.6.1
```

  * Merge the branch back into `develop`:
```
git checkout develop
git merge --no-ff hotfix-0.7.6.1
```
  * Delete the `hotfix` branch:
```
git branch -d hotfix-0.7.6.1
```

# Code development and debugging #

In addition to the standard requirements, a developer should probably also install [cython](http://cython.org). In some cases it might be necessary to have [SWIG](http://www.swig.org) as well.

Developers should be aware of how the `setup.py` script handles Cython code (see [Issue 85](https://code.google.com/p/mdanalysis/issues/detail?id=85)):

  * If **no Cython**, `setup.py` uses all the `.c` files that are included in the standard MDAnalysis distribution. If you _changed any `.pyx` files_ then it will _still use the old files_!
  * If **Cython is installed**, it is used to compile extension and `.pyx` source files are used instead of `.c` files.
  * From there, `.pyx` files are converted to `.c` files if they are newer than the already present `.c` files _or_ if the `--force` flag is set (e.g `setup.py build --force`).

By doing so, end users (or devs) should not trigger the `.pyx` to `.c` conversion since `.c` files delivered with source packages are always up to date. But devs who work on the `.pyx` files will automatically trigger the conversion since `.c` files will then be outdated.
Using the `--force` flag with Cython installed will also trigger the conversion though.