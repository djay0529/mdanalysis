Until 2011-11-28, the source code was stored in a **subversion repository** but was [converted to git](svn2gitConversion.md). The SVN repositoryrepository still exists and is full accessible, using any of the instructions in this section. However, all development is done in the [git repository](Source.md) using [git](git.md).

**DO NOT USE THE svn REPOSITORY** — see the [notes on the git repository](Source.md) instead.

Below are some of the old notes on how to use the repository (geared towards developers).

# Subversion #

[Subversion](http://subversion.apache.org/) (`svn`) is the native (though somewhat limited) format for the MDAnalysis repository. Follow the [Source Checkout Instructions](http://code.google.com/p/mdanalysis/source/checkout).

Note that the password that `svn` asks for is _not_ your gmail password but the [googlecode.com generated one](http://code.google.com/hosting/settings).


# git-svn #

[git](http://gitscm.org/) is a very modern, distributed source code management system. There are advantages to using it, even in conjunction with a svn repository.

Quick notes on how to this with googlecode:

First make a file to translate commitor names to persons:
```
cat >AUTHORS.txt <<EOF
oliver = Oliver Beckstein <orbeckst@jhmi.edu>
orbeckst = Oliver Beckstein <orbeckst@gmail.com>
denniej0 = Elizabeth Denning <denniej0@gmail.com>
denniej0@gmail.com = Elizabeth Denning <denniej0@gmail.com>
naveen.michaudagrawal = Naveen Michaud-Agrawal <naveen.michaudagrawal@gmail.com>
nmichaud = Naveen Michaud-Agrawal <naveen.michaudagrawal@gmail.com>
root   = Oliver Beckstein <orbeckst@gmail.com>
Danny.Parton = Danny Parton <danny.parton@gmail.com>
philipwfowler = Philip Fowler <philipwfowler@googlemail.com>
tyler.je.reddy = Tyler Je Reddy <tyler.je.reddy@gmail.com>
tyler.je.reddy@gmail.com = Tyler Je Reddy <tyler.je.reddy@gmail.com>
joseph.goose = Joseph Goose <joseph.goose@bioch.ox.ac.uk>
jandom = Jan Domanski <jandom@gmail.com>
EOF
```
Clone the repository; this takes a while but only has to be done once:
```
git svn clone -A AUTHORS.txt --stdlayout --username USERNAME https://mdanalysis.googlecode.com/svn mdanalysis
```
This will pull in the whole change history and build a git repository.

Typical svn-like use:
```
git svn rebase       # pull changes from svn repository = "svn update"

# edit FILE1 FILE2 ...

git status           # show changes (svn status)

git add FILE1 FILE2  # add (svn add) or mark files for inclusion in next commit (-/-)

git commit -m "fixed the flux-flobberer"  # commits LOCALLY in git

# ... do more work locally, add, commit

# finally, upload changes to googlecode
git svn dcommit      # all changes since rebase are uploaded (svn commit)
```


You can then git to make _local_ branches, rewrite _local_ history etc. Just follow some simple rules:
  * The git "master" should correspond to the svn "trunk".
  * Do not `git-merge` any branches that are in the svn repository. Only merge _local_ branches into the master (trunk).
  * Do not clone the git repository itself. Always `git svn clone` the googlecode svn repository; otherwise odd things might happen.
  * Use `git-cherrypick` to port patches from one svn branch to another (e.g. port last-minute fixes from release branches to trunk).
  * Use `git svn dcommit -n` to check that you are submitting to the correct branch.
  * Only rewrite _local_ history (`git-rebase`); _local_ means anything that has not appeared in the googlecode repository yet (i.e. before a `git svn dcommit`).

Post questions and comments below.