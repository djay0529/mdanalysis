As oulined in [Issue 223](https://code.google.com/p/mdanalysis/issues/detail?id=223), Google Code is closing down and we need to move the MDAnalysis project. The (current) choice is GitHub. This page collects the issues we need to consider. Important documentation on the whole procedure can be found in the [GitHubExporterFAQ](https://code.google.com/p/support-tools/wiki/GitHubExporterFAQ).

# Transition #
We'd like to do a **clean transition** where we
  1. stop using Google code
  1. transition to GitHub
  1. then start using GitHub.

From reading the [issues for export](https://code.google.com/p/support-tools/issues/list) our best hope appears to be to use the export wizard [Export to GitHub](https://code.google.com/export-to-github/) (instead of [manual tools](https://code.google.com/p/support-tools/wiki/GitHubExporterFAQ)) but the wizard cannot update an existing repository and so we want to avoid having to manually sync later changes from Google Code to Git Hub.

# TASKS by Person #
  * [Issue Tracker](#Issue_Tracker.md) ([Issue 231](https://code.google.com/p/mdanalysis/issues/detail?id=231)): **richardjgowers** (Richard) will
    1. check if there are any [open issues](https://code.google.com/p/mdanalysis/issues/list) that have attachments. If yes, then
    1. download the attachments from all open issues and store them in individual directories named _IssueNNN/CommentMM_ with the original file names (_NNN_ is the issue id number, eg 055 for [Issue 55](https://code.google.com/p/mdanalysis/issues/detail?id=55) and _MM_ is the comment number, eg 01 for comment [#1](https://code.google.com/p/mdanalysis/issues/detail?id=55#c1) or 00 for the initial issue report).
    1. prepare a zip file (and wait for further instructions until we figure out a way to put everything back together)
  * team ([Issue 232](https://code.google.com/p/mdanalysis/issues/detail?id=232)): **orbeckst** (Oliver)
    1. collect GitHub usernames from all current contributors
  * [Repository](#Repository_Organization.md) ([Issue 233](https://code.google.com/p/mdanalysis/issues/detail?id=233)): **orbeckst** (Oliver) will
    1. remove commit access to Google Code from everyone
    1. create a final snapshot (via Takeout) of the project and save the admin and people data
    1. use the Export Wizard to transfer the repository to GitHub (code history, issue tracker (without attachments), wiki)
    1. split off applications into individual repositories
    1. set up new access lists (based on the currently active committors)
  * [Wiki](#Wiki.md) ([Issue 234](https://code.google.com/p/mdanalysis/issues/detail?id=234)): **TBN** will
    1. turn the wiki branch into a GitHub wiki (see [Wiki](#Wiki.md))
    1. manually fix up pages where necessary
    1. start filing issues re: wiki where community cleanup is needed
  * Online docs (development branch) ([Issue 235](https://code.google.com/p/mdanalysis/issues/detail?id=235)): **TBN** will
    1. devise a workflow that allows us to publish the html docs as a gh-pages branch in the repository
  * MDAnalysis home page ([Issue 236](https://code.google.com/p/mdanalysis/issues/detail?id=236)): **TBN**
    1. Set up https://mdanalysis.github.io/ as a [GitHub pages](https://pages.github.com/) site using either pure html or Jekyll (see the preliminary notes on [web site development](https://github.com/MDAnalysis/MDAnalysis.github.io#mdanalysis-web-site)).
    1. populate and maintain the site


# Repository Organization #
On GitHub, the project will be owned by the Github Organization [MDAnalysis](https://github.com/MDAnalysis). Using an organization will allow us to maintain multiple repositories with appropriate user permissions.

## Required GitHub Repositories ##
We will initially set up repositories for
  * **mdanalysis**
    * package
    * testsuite
    * maintainer
    * branches:
      * master
      * develop
      * gh-pages (will contain the docs and we will need to find a way to make this relatively painless to update – but see also [Issue 183](https://code.google.com/p/mdanalysis/issues/detail?id=183))
      * feature branches that are still required for ongoing development
  * **applications**
    * split off from current repository by [detaching a subdirectory into a separate git repository](http://stackoverflow.com/questions/359424/detach-subdirectory-into-separate-git-repository/17864475#17864475)
    * will only affect [RotamerConvolveMD](RotamerConvolveMD.md) at the moment
  * **tutorials**
    * move [MDAnalysisTutorial](https://github.com/orbeckst/MDAnalysisTutorial) there

## Tasks ##
  * prune feature branches on google code:
    * Which feature branches are still important?
    * Remove those that are fully merged or will not be developed further (check with commit authors)
  * move the following components to GitHub
    1. [Code](#Code.md) (mdanalysis repository)
    1. [Wiki](#Wiki.md) (wiki repository)
    1. [Issue Tracker](#Issue_Tracker.md)
    1. home page contents
      * summary
      * people
      * links (e.g. to mailing lists)
    1. downloads

> Points 1-3 _should_ be handled by the Google Code export wizard although the [FAQ](https://code.google.com/p/support-tools/wiki/GitHubExporterFAQ) also shows how to do this manually.

> Point 4 needs to be done manually (easiest is to snapshot the [admin page](https://code.google.com/p/mdanalysis/admin)); the [contributors](https://code.google.com/p/mdanalysis/people/list) page will be snapshot, too. All the information is probably also in the  [Takeout](https://www.google.com/settings/takeout) JSON file but that needs to be verified).

> Point 5 needs to be done manually by getting (wget/curl) the individual tarballs from the [old download page](https://code.google.com/p/mdanalysis/downloads/list); perhaps we put them up on PyPi just for historical reasons  (if this is possible).


## Procedure ##
We plan to use the export to GitHub wizard (new button in the repo). Oliver (orbeckst) will do this (as a owner). It will be initially moved to [orbeckst](https://github.com/orbeckst)'s personal GitHub space (a limitation of the export procedure) and then [transferred inside GitHub](https://help.github.com/articles/transferring-a-repository/) to the [MDAnalysis](https://github.com/MDAnalysis) Organization.

Oliver can also use [Google Takeout](https://www.google.com/settings/takeout) to obtain a big JSON file that contains all the project metadata (including the issue tracker except attachments). This provides a fall-back for manual transfer.


# Issue Tracker #
  * attachments will not be transferred
    * need to manually save any attachments that we want to keep
    * assign one person to go through the open issues and
      * download attachments
      * keep a note of issue and comment number
  * issue owners and commenters might not be tranferred; in principle I  have this information in the Takeout dump but we cannot change this  information on GitHub.


# Code #
  * All code **should** transfer cleanly to GitHub.
  * We will need to split off **applications** from the current repository
    * extract the commits from the history (or just start fresh if too difficult, will only affect RotamerConvolveMD at the moment)
  * There's a possibility of [mapping usernames to GitHub users during  the import process](https://code.google.com/p/support-tools/issues/detail?id=49) but it seems fragile. Not sure if it is needed.

# Wiki #
  * By default, the wiki will be converted to markdown and placed in a wiki branch, as described in the FAQ: [here did my Google Code wikis go?](https://code.google.com/p/support-tools/wiki/GitHubExporterFAQ#Where_did_my_Google_Code_wikis_go?).
  * Will the wiki be transformed into a GitHub wiki? **No**, but according to the FAQ there exists a tool [finishGoogleCodeGitHubWikiMigration](https://github.com/morgant/finishGoogleCodeGitHubWikiMigration) to move the wiki files from the wiki branch into the repo's wiki section.