_This page is currently only a quick selection of notes. Feel free to clean it up and make it more usable for everyone._

Note: Please also check the [repository structure](RepositoryStructure.md) and the [development workflow](DevelopmentWorkflow.md).

[git](git.md) allows us to use a _distributed development model_.

  1. **Anyone** can simply **[clone the repository](http://code.google.com/p/mdanalysis/source/clones)** and work on the clone.
  1. Once the code in the clone is ready to be integrated in the main development line, **send a "pull" request** to the [mdnalysis-devel list](http://groups.google.com/group/mdnalysis-devel) so that one of the core developers can review the changes and integrate them.

## Example ##
fullofgrace88 wants to tackle [Issue 2](https://code.google.com/p/mdanalysis/issues/detail?id=2) so they created the clone [fullofgrace88-mdanalysis-tpr-reader](http://code.google.com/r/fullofgrace88-mdanalysis-tpr-reader/). They will work on the clone.

A core developer can create a separate topic branch (here named "Issue/2") in their own local repository for testing anything that comes from the clone:

```
git remote add fullofgrace88 https://code.google.com/r/fullofgrace88-mdanalysis-tpr-reader 
git fetch --all
git checkout -b Issue/2 fullofgrace88/master
```

Once this is done, the developer can switch to the topic branch with
```
git checkout Issue/2
```
(Switching back to the main development line is `git checkout develop`.)

Once a **pull request** comes in, the developer does
```
git checkout Issue/2
git pull
```
and reviews/test the changes. If all goes well, the changes are merged into the development branch with
```
git checkout develop
git merge Issue/2
```
and eventually pushed to the main repository with `git push`.