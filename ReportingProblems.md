MDAnalysis is an open source project, which means that anyone in its user community is free to use, copy, modify, and distribute the code. Invariably, some things do not work as expected or an essential feature is missing – this is unfortunate but one has to remember that everyone is contributing their time and effort on a purely voluntary basis. No-one is getting paid to work on MDAnalysis. However, anyone who uses the library for their own work has a vested interest in the code producing correct results and having useful features. Therefore, everyone involved **really likes to get feedback** on
  * what is not working (**bugs**), and
  * what would make MDAnalysis a better package (**feature requests**)

So: If you're using MDAnalysis and something does not work as expected or if you can think of a neat improvement then **talk to us** – where "us" really means the whole MDAnalysis community, of which _you_ are already a part by taking an interest in the package.


## Getting in touch ##
The best way to report a problem is the [Issue Tracker](#Issue_Tracker.md) and for general questions related to [Installation](Install.md) or using the library ask on the [mailing list](#Mailing_List.md).

### Issue Tracker ###
To report **bugs** or **feature requests** enter an **Issue** in the **[Issue Tracker](https://code.google.com/p/mdanalysis/issues/list)**.

The developers monitor it closely and you typically get a reaction in less than two days.

Remember to
  * "star" the issue (highlight the star icon): You will get automatic email updates whenever anything is added to your report. This is really crucial because often developers have additional questions and cannot proceed without your answers or specific test cases.
  * Tell us
    1. What you were doing, in particular write down any code that reproduces your problem.
    1. What you expected to happen, e.g. what the correct result or outcome is.
    1. What actually happened.
    1. Any error messages, stack traces or other information. (Note that you should enable logging with `MDAnalysis.start_logging()` and also look into the logfile (by default, "MDAnalysis.log").)

#### Test cases ####
The best bug reports are the ones that a developer can reproduce on their own machine – then it is easy to figure out very quickly what goes wrong. Thus, any bug report that provides _all information to reproduce the bug_ is often fixed quickly.

If you can, [include a test case](http://code.google.com/p/mdanalysis/wiki/ContributingCode#Test_cases) with your bug report:
  * The Python code to reproduce the bug.
  * Any data file that might be needed (as an attachment to the Issue ticket).
    * Keep the _file size small_ or see if you can reproduce it with one of the data files already included (`MDAnalysis.tests.datafiles`)
    * Do _not submit any confidential data_ because the issue tracker (and the mailing list) is public and hence anyone can read the files you submit.
    * We might want to include your file as a test case in the UnitTests. Therefore, by posting it in the issue tracker you publish any data or code under the _GNU Public Licence, version 2 (or higher)_ or a [compatible licence](http://www.gnu.org/licenses/license-list.html#GPLCompatibleLicenses) (then mention this licence explicitly). If you do not want this then do not post the data but understand that in this case it might be hard or impossible to address the issue.

### Mailing List ###
The project has a friendly mailing list on Google Groups named [mdnalysis-discussion](http://groups.google.com/group/mdnalysis-discussion) (no typo). You can post questions through the web interface  or subscribe to it.

The users mailinglist welcomes all MDAnalysis users. If you have a problem for which you cannot find a solution
  * in the [documentation](http://mdanalysis.googlecode.com/git-history/develop/package/doc/html/index.html)
  * the [wiki](https://code.google.com/p/mdanalysis/w/list)
  * using the [mailing list search](https://groups.google.com/forum/?fromgroups#!forum/mdnalysis-discussion)
then go ahead and ask.

In particular, there's always a helpful answer to be had for questions about [Installation](Install.md) or using the library.

### Wiki comments ###
You can also comment on Wiki articles using the comment field at the bottom. The comments are automatically posted to the mailing list.

## Further reading ##
Have a look at the excellent [How to report bugs effectively](http://www.chiark.greenend.org.uk/~sgtatham/bugs.html) (yes, it's long; skip to the Summary at the end if in a hurry) and [How to ask questions the smart way](http://www.catb.org/~esr/faqs/smart-questions.html).