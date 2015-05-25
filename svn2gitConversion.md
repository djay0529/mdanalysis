As described in [Issue 86](https://code.google.com/p/mdanalysis/issues/detail?id=86), we wanted to upgrade our source code management system from [subversion](subversion.md) to [git](git.md). The upgrade took place on 2011-11-28 and this page describes what was done.

orbeckst basically followed the [Convert your project from Subversion to Git](http://code.google.com/p/support/wiki/ConvertingSvnToGit) instructions with some additional steps described below.

The standard git instructions including the link to the username/password (for committers) is on the [Source: Checkout](http://code.google.com/p/mdanalysis/source/checkout) instructions page. I set up the appropriate `.netrc` file so that git handles the authentication automatically:
```
machine code.google.com login USERNAME@gmail.com password GOOGLECODE-PASSWORD
```
with the [[generated googlecode.com password](http://code.google.com/hosting/settings).

A list of username/author mappings (`AUTHORS.txt`) was taken from our previous [git-svn instructions](Source#git.md).

## Wiki ##
Converting the wiki was straightforward:
```
git svn clone -A AUTHORS.txt https://mdanalysis.googlecode.com/svn/wiki mdanalysis.wiki
cd mdanalysis.wiki
git remote add googlecode https://code.google.com/p/mdanalysis.wiki
git push --all googlecode
```

## Code ##
The trunk was converted with the same procedure:
```
git svn clone -A AUTHORS.txt --stdlayout --username orbeckst https://mdanalysis.googlecode.com/svn mdanalysis
cd mdanalysis
git remote add googlecode https://code.google.com/p/mdanalysis
git gc    # optional garbage collection and compaction of repo
git push --all googlecode
```

The svn tags were converted to real git tags with the snippet provided by Azatoth's comment at [convert git-svn tag branches to real tags](http://gitready.com/advanced/2009/02/16/convert-git-svn-tag-branches-to-real-tags.html):
```
git for-each-ref refs/remotes/tags --shell --format="r=%(refname:short) t=\${r#tags/}" | while read e; do 
   eval "$e"; 
   git tag -f $t refs/remotes/$r; 
   git branch -d -r $r; 
done 
```
and then pushed
```
git push --tags googlecode
```

The procedure did not create the _branches_ of the subversion repository in the new git repository. The following is probably not the most elegant/canonical way to do this but it seems to work (feel free to comment below). First list all remote branches:
```
git branch -r
```
We really only need the ''MDAnalysisTestData'' (see [Issue 87](https://code.google.com/p/mdanalysis/issues/detail?id=87)) so we check out that branch and then push it:
```
git checkout -b MDAnalysisTestData MDAnalysisTestData
git push MDAnalysisTestData
git checkout master
```
(If any of the other branches should ever be needed then they can be accessed through the old svn repository, which will be kept.)

## Results ##
The whole procedure appears to be successful

  * I can clone the whole repository.
```
git clone https://code.google.com/p/mdanalysis/
```
> and switch to the test data branch:
```
git checkout -b MDAnalysisTestData origin/MDAnalysisTestData
```
> and back to the master
```
git checkout master
```
  * The [online docs](http://mdanalysis.googlecode.com/git/doc/html/index.html) appear to be functional.


## Issues ##
If any serious issues appear, [open an issue on the tracker](http://code.google.com/p/mdanalysis/issues/list).
  * The [source code](http://code.google.com/p/mdanalysis/source/browse/) browser does not show log history for many files.

