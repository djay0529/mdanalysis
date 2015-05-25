MDAnalysis is a free and open source project. It evolves and grows with the demands of its user base. Therefore, the development team very much welcomes contributions from its users. Contributions can take many forms, for instance
  * **bug reports** or **enhancement requests** filed through the [Issue Tracker](http://code.google.com/p/mdanalysis/issues/list)
  * **source code** for fixing bugs or new functionality (e.g. new analysis tasks)
  * **documentation** (either in the code or on the [wiki](http://code.google.com/p/mdanalysis/w/list), both on pages and in comments)
  * **discussions** on the [mdnalysis mailing list](http://groups.google.com/group/mdnalysis-discussion)


## Contributing code ##

Maybe you have a useful new feature for MDAnalysis (for instance, you wrote a specialized analysis task for a paper and now, as the paper has been accepted, you want to share the code) or a fix for a bug or a performance improvement. How can you get this code into the public version of MDAnalysis so that other users can also benefit from your code (and possibly cite you...) or you are just tired adding your feature every time a new release of MDAnalysis comes out.

If you are not listed as a _committer_ under [Project People](http://code.google.com/p/mdanalysis/people/list) then you cannot just add code to the source code repository. However, because we are using [git for source code management](Source.md), things are still very easy: The typical approach for getting code into MDAnalysis follows a simple protocol:

  1. Ensure that you can publish your code under a **licence that is compatible with the GNU Public Licence 2 (or higher)**; otherwise we will not be able to incorporate the code into MDAnalysis.
  1. Read the [how MDAnalysis is developed](DevelopmentWorkflow.md)
  1. **[Clone the MDAnalysis source](http://code.google.com/p/mdanalysis/source/clones)**. [Distributed development with git](DistributedDevelopment.md) describes in more detail how to do this.
  1. **Add your changes** to _your own clone_ of MDAnalysis. **Push** the changes so that they show up in the clone. (Also note the section on [Test cases](#TestCases.md) below.)
  1. File an **enhancement request** for a new feature or a standard **defect report** on the [Issue Tracker](http://code.google.com/p/mdanalysis/issues/list).
    * describe the nature of the code you're contributing
    * paste the link to your clone
    * add any other important information
  1. **Monitor your issue report** for additional questions or suggestions (e.g. by starring it and [setting your preferences](https://code.google.com/hosting/settings) so that email is sent to you when changes occur). If you don't get a response in three days, send an email to the mailing list, announcing your contribution (with a link to the issue report).

A developer will then review your code and likely incorporate it into mainline MDAnalysis by pulling the changes from your clone.

Once your changes have been added, we will
  * add your name to the list of [AUTHORS](http://code.google.com/p/mdanalysis/source/browse/AUTHORS) and add you as a _contributor_ to the [list of People](http://code.google.com/p/mdanalysis/people/list)
  * add a short description of your changes to the [CHANGELOG](http://code.google.com/p/mdanalysis/source/browse/CHANGELOG) (typically just the title of your issue report and the issue number)
  * add the _citation information_ of any paper that you want users to reference to the appropriate files and the website
  * close the issue report

If you prefer to not be listed then we will certainly honor your preference.

Note that by pulling your changes, the source code itself is still associated with your name: Anyone looking at the source will be able to see that this was _your_ contribution to this open source project.

Please feel free to ask questions either on the mailing list or through the comments at the bottom of the page.

## Test cases ##
Whenever you contribute a new feature or fix/enhance existing code then you should create an appropriate [test case](UnitTests.md) (also known as UnitTests) that exercises your code. This is very important because
  1. it ensures that your code works as expected and more importantly,
  1. in the future we can always test that it is still working correctly.
UnitTests are a crucial component of proper software engineering (see e.g. [Software Carpentry on Testing](http://software-carpentry.org/4_0/test)) and a large (and growing) test suite is one of the strengths of MDAnalysis.

Test-driven development is actually a very good way to write code: You _first_ write your test case and while development you repeatedly run your tests until they pass.

UnitTests has more information on the testing framework. However, you might also want to look at existing test cases (all to be found in [testsuite/MDAnalysisTests](http://code.google.com/p/mdanalysis/source/browse/#git%2Ftestsuite%2FMDAnalysisTests)) to get an idea how to structure your test cases.

### When are test cases required ###
For some contributed code we require test cases in order to integrate it with the full library. We do this in order to make sure that our users can rely on the library being as bug-free as possible.

  * New code and enhancements for the **core of MDAnalysis** i.e. in **MDAnalysis/core, MDAnalysis/topology, MDAnalysis/coordinates, MDAnalysis/selections, MDAnalysis/KDTree** _must_ always come with sufficient test cases.
  * Code for the **analysis** module (MDAnalysis/analysis) should be accompanied with test cases. (In the future this might become a hard requirement.)

### New data files ###
If possible, re-use the existing data files in MDAnalysis (see [MDAnalysisTests.datafiles](http://code.google.com/p/mdanalysis/source/browse/testsuite/MDAnalysisTests/datafiles.py) and the files in the directory [testsuite/MDAnalysisTests/data](http://code.google.com/p/mdanalysis/source/browse/testsuite/MDAnalysisTests/#MDAnalysisTests%2Fdata)) for tests; this helps to keep the (separate) MDAnaltsisTests package small. If new files are required (e.g. for a new coordinate Reader/Writer) then
  1. Use small files (e.g. trajectories with only a few frames and a small system).
  1. Make sure that the data are _not confidential_ (they will be available to everyone downloading MDAnalysis) and also be aware that by adding them to MDAnalysis you _licence these files under the [GNU Public Licence v2](http://www.gnu.org/licenses/gpl-2.0.html)_ (or a compatible licence of your choice — otherwise we cannot include the files into MDAnalysis).
  1. Add the files to the `testsuite/MDAnalysisTests/data` directory and appropriate file names to `testsuite/MDAnalysisTests/datafiles.py`.

Make sure that your test case runs and that _all other test cases are still passing_.