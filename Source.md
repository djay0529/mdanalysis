Note about the source code management system used for MDAnalysis:
> As of 2011-11-28, the MDAnalysis source code (and the wiki) is managed in a [git](http://gitscm.org/) repository.
> The original [repository subversion repository](SubversionRepository.md) is hence deprecated and all code should be [converted to git](svn2gitConversion.md).

## Git repository structure ##
The full description of the git repository structure is available [here](RepositoryStructure.md).

Follow the [source checkout instructions](http://code.google.com/p/mdanalysis/source/checkout) if you want to clone/download/checkout the repository.

### Stable source code ###
Production-ready code is stored in the **[main branch](RepositoryStructure#The_main_branch.md)**.

You can browse this code [here](http://code.google.com/p/mdanalysis/source/browse/?name=master).

Please note that main development is done using the **[develop branch](RepositoryStructure#The_develop_branch.md)**.

### Development source code ###
Development code is stored in the **[develop branch](RepositoryStructure#The_develop_branch.md)**.

You can browse this code [here](http://code.google.com/p/mdanalysis/source/browse/?name=develop).

## Source code organisation ##
When you check out the source code you'll get a directory hierarchy that [looks like](http://code.google.com/p/mdanalysis/source/browse/)
```
mdanalysis/README
           package/
           testsuite/
```
  * [package](http://code.google.com/p/mdanalysis/source/browse/#git%2Fpackage) contains the _MDAnalysis toolkit_ source code.
  * [testsuite](http://code.google.com/p/mdanalysis/source/browse/#git%2Ftestsuite) contains the code for the UnitTests and the datafiles

When you [add a new feature](ContributingCode.md) (in `package`) then you should also add a test case (see UnitTests) and possibly data files in `testsuite` and commit both together.

(See also [Issue 87](https://code.google.com/p/mdanalysis/issues/detail?id=87).)