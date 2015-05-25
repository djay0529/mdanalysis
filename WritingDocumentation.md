# Contents #


# New Guidelines Proposal (March 2014) #
In general, we are proposing (still open to some debate) to adjust the MDAnalysis documentation to match the formatting standards adopted by numpy / scipy as described in the [documentation standard](https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt). Much of the scientific Python community adopts this standard (numpy / scipy / scikit) and matplotlib appears to be moving in this direction as well with [MEP10](https://github.com/matplotlib/matplotlib/wiki/Mep10). Arguably, new MDAnalysis users from the scientific Python community would appreciate documentation that is consistent with other major scientific libraries, and this format seems to work quite well.

There's a lot to be said for clear and concise documentation--it can save the users and developers time and attract new users (and eventually developers). It's quite nice to be able to google a function or class and obtain a single clear page with its documentation in a familiar format.

The format generally involves a single class / function per documentation page, with example usage strongly encouraged (see, for example: [scipy.spatial.ConvexHull](http://docs.scipy.org/doc/scipy-dev/reference/generated/scipy.spatial.ConvexHull.html)). The order and formatting of sections is perhaps best understood by looking at the [example](https://github.com/numpy/numpy/blob/master/doc/example.py) provided in the numpy documentation standard. In general, we want to avoid large / bloated documentation pages with several different functions / classes. If appropriate, common classes and functions can be associated in a summary table as shown for [scipy.spatial.distance](http://docs.scipy.org/doc/scipy/reference/spatial.distance.html).

There are certainly some points to dicuss: 1) will we adopt a strict [doctest](http://docs.python.org/library/doctest.html) format for examples in docstrings (not meant to replace unit tests)? 2) Will we adopt the [numpydoc](https://pypi.python.org/pypi/numpydoc) Sphinx extension? This would be needed for a more precise match to the numpy documentation standard. 3) other issues?

While I've tried to be reasonably consistent in my recently-added modules (i.e., [3D streamplot](http://mdanalysis.googlecode.com/git-history/develop/package/doc/html/documentation_pages/visualization/streamlines_3D.html)), I'm also guilty of cutting corners in a few of the formatting areas. The idea is to make changes that provide increased consistency with the numpy documentation standard if you stumble across a module that you think could use enhanced documentation.


# Overview #
MDAnalysis has a lot of documentation in the Python doc strings. In the Python interpreter one can simply say
```
  import MDAnalysis
  help(MDAnalysis)
  help(MDAnalysis.Universe)
```
In `ipython` one can use the question mark operator
```
  MDAnalysis.Universe ?
```
Interactive use is not always convenient, though, and hence [Issue 26](https://code.google.com/p/mdanalysis/issues/detail?id=26) called for proper documentation.

As of Jan 2011, the **[Online Documentation](http://mdanalysis.googlecode.com/git/doc/html/index.html)** is available, too.

This page should help developers write good documentation that can be easily processed into online docs.



## Guidelines ##

When writing Python code, you should always add a doc string to each public (user-visible)
  * module
  * class
  * method
  * function
(We consider something public if the name does not start with an underscore.)
The module doc string can be a short sentence describing what the module does or a long document including examples and references. The other doc strings should generally allow a user to use the code element, so they need to say what the code does, what input it requires, and what it returns. Additionally, one should document the exceptions raised and known bugs or quirks.

## Location of documentation ##
  * Code is documented through Python doc strings (see [PEP 257](http://www.python.org/dev/peps/pep-0257/) for good conventions); thus each Python file should contain some documentation.
  * The [Online Documentation](http://mdanalysis.googlecode.com/git/doc/html/index.html) is generated from [reStructuredText](http://docutils.sourceforge.net/rst.html) in [doc/sphinx/source/documentation\_pages](http://code.google.com/p/mdanalysis/source/browse/#git%2Fpackage%2Fdoc%2Fsphinx%2Fsource%2Fdocumentation_pages).
  * The googlecode pages also hold documentation in the [MDAnalysis Wiki](http://code.google.com/p/mdanalysis/w/list) (but as far as the user docs go, they are being phased out in favour of the [Online Docs](http://mdanalysis.googlecode.com/git/doc/html/index.html)).


# Online Docs format: sphinx reStructuredText #

## Sphinx basics ##
We are using [reStructuredText](http://docutils.sourceforge.net/rst.html) ("rst" or "reST") in the Python code and in the [Online Documentation](http://mdanalysis.googlecode.com/git/doc/html/index.html). The reST is processed with [sphinx](http://sphinx.pocoo.org/) to generate HTML or PDF. Thus, the docs should use reST with the sphinx autodoc extensions:
  * sphinx [reST Primer](http://sphinx.pocoo.org/rest.html)
  * sphinx [autodoc extension](http://sphinx.pocoo.org/ext/autodoc.html)

Note that each page of the  [Online Documentation](http://mdanalysis.googlecode.com/git/doc/html/index.html) has a link to the _Source_ of the page. You can look at it in order to find out how a particular page has been written in reST and copy the approach for your own documentation.

## Generating the docs ##
In order to **generate the documentation**, one has to have sphinx installed. Then one generates the html docs with
```
cd doc/sphinx
make html
```
This generates and updates the files in `doc/html`.

For testing, you can also use
```
python setup.py build_sphinx
```
which creates docs under `build/sphinx/html` but these files are never committed to the repository so remember to use the procedure with `make` to generate online docs.

Generated html is checked into the git repository. Newly generated files _must_ be added (`git add`) or they don't show up in the online docs!

## Mathematics ##
We are using MathJax with sphinx so you can write LaTeX code in math tags, e.g.
```
#<SPACE if there is text above equation>
.. math::
   e^{i\pi} = -1
```
or inline
```
We make use of the identity :math:`e^{i\pi} = -1` to show...
```

Note that you should _always_ make doc strings with math code **raw** python strings **by prefixing them with the letter "r"**:
```
def rotate(self, R):
    r"""Apply a rotation matrix *R* to the selection's coordinates.

    :math:`\mathsf{R}` is a 3x3 orthogonal matrix that transforms a vector
    :math:`\mathbf{x} \rightarrow \mathbf{x}'`:

    .. math::

       \mathbf{x}' = \mathsf{R}\mathbf{x}
    """
```
or else you will get problems with backslashes in unexpected places (see [Stakcoverflow: Mathjax expression in sphinx python not rendering correctly](http://stackoverflow.com/questions/16468397/mathjax-expression-in-sphinx-python-not-rendering-correclty")).

# Serving documentation from the git repository #
The [current Online Docs](http://mdanalysis.googlecode.com/git/package/doc/html/index.html) are served directly from the git repository and thus reflect the development version of the code. On the home page we also link to a static version of the pages which lives on the Python package index ([latest release online docs](http://packages.python.org/MDAnalysis/)).

Serving of the current docs works because Google does some mime magic on "raw" urls (see the [Google GitFAQ](http://code.google.com/p/support/wiki/GitFAQ#Thu%20Sep%20%208%2009%3A48%3A54%202011) and my question somewhere in the comment) and hence it "should just work" when you
  * provide the appropriate URL such as http://mdanalysis.googlecode.com/git/doc/html/index.html ,
  * and just name images, CSS, javascript etc with the correct file extension

(See [Issue 86](https://code.google.com/p/mdanalysis/issues/detail?id=86) for a discussion.)