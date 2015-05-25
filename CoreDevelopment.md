If you work on code in the core of MDAnalysis then a few additional rules apply in addition to the [general rules for contributing code](ContributingCode.md).

The core of MDAnalysis consists of files under
  * **MDAnalysis/core**
  * **MDAnalysis/topology**
  * **MDAnalysis/coordinates**
  * **MDAnalysis/selections**
  * **MDAnalysis/KDTree**
  * associated C and Cython code under **src**

Basically we want to make sure that the new code does not break the existing one and that

  1. [Make a clone](https://code.google.com/p/mdanalysis/source/clones) of the latest development branch in google code .
  1. Pull the latest changes from the development branch.
  1. Merge your code into this branch.
  1. Push the changes to your public clone. Let us know through the developer list that we can pull your changes to look at them and test them.

Before asking us to pull check the following:

  * Run the unit tests: they should all pass (or at least pass as well as the unmodified develop branch)
  * Have you added [test cases](UnitTests.md) for your own code? Check that your tests pass with your changes (and fail without them).
  * Have you [added documentation](https://code.google.com/p/mdanalysis/wiki/WritingDocumentation) (Python doc strings) that describe your new feature?
  * Have you updated the CHANGELOG (and AUTHORS when you're contributing your first commit)?
  * Have you committed changes in self-contained commits (ideally, one feature per commit) with a good, expressive log message that references any [Issues](https://code.google.com/p/mdanalysis/issues/list) that you've been working on?