# Git repository structure #

As introduced by [issue 96](https://code.google.com/p/mdanalysis/issues/detail?id=96), the structure adopted for the MDAnalysis source code central repository is based on the model which is fully described here: http://nvie.com/posts/a-successful-git-branching-model/.

This model relies on two main branches:
  * a **master** branch containing _production-ready_ code.
  * a **develop** branch that reflects the latest delivered development changes for the next release.

In addition to these two branches with an infinite lifetime, three kinds of supporting branches may also used occasionally:
  * **feature** branches.
  * **release** branches.
  * **hotfix** branches.

## The master branch ##

The **[master](http://code.google.com/p/mdanalysis/source/list?name=master)** branch should always reflect a _production-ready_ state. In other words, this branch is used to "store" releases so an end-user cloning this branch will have 1. a stable code and 2. the "raw and unpackaged" version of the last stable release.

## The develop branch ##
The **[develop](http://code.google.com/p/mdanalysis/source/list?name=develop)** branch is the main development. This branch should not contain highly experimental code but should rather be considered as an "integration branch": when code is stable enough to be added to the next release, it may be added to this branch.

## The supporting branches ##
As opposed to the **main** or **develop** branches, the supporting branches are used (more or less) temporarily. Their goal is to aid parallel development between team members by:
  * easing tracking of features. (**feature** branches)
  * preparing for production releases. (**release** branches)
  * assisting in quickly fixing live production problems. (**hotfix** branches)

### The feature branches ###

The **feature** branches are used to develop new features for the upcoming or a distant future release. A typical **feature** branch would exist as long as the feature is in development and should be deleted when the code is merged into the **develop** branch (or if the feature is discarded).

  * May branch from: **develop**
  * _Must_ be merged into: **develop**
  * Naming convention: about anything except `master`, `develop`, `release-*`, `hotfix-*`


### The release branches ###

The **release** branches are used to prepare a new production release. They allow last-minute fixes and verifications to make sure everything is OK before a "public" release.

  * May branch from: **develop**
  * _Must_ be merged into: **develop** and **master**
  * Naming convention: `release-*`

### The hotfix branches ###

The **hotfix** branches are used typically used when a critical bug is found in a production release. Everything related to the resolution of the bug should be handled in a **hotfix** branch and then propagated to **develop** and **master**.

  * May branch from: **master**
  * _Must_ be merged into: **develop** and **master**
  * Naming convention: `hotfix-*`