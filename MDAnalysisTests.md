The **MDAnalysisTests** package contains the UnitTests that are used to check that MDAnalysis is performing to specifications. The package also contains real-life data files that we use to run many of the tests.

These tests are a very important part of MDAnalysis because by running them we can ensure that enhancements or bug fixes do not introduce new problems. Having a large test suite (more than 400 tests at the moment and growing) is a strength of MDAnalysis.

In order to run the UnitTests, **MDAnalysisTests** must be installed together with **MDAnalysis** itself. Although it is not required, any user who relies on the output from MDAnalysis for their own research is _strongly encouraged to install MDAnalysisTests_ and run the UnitTests in order to check that the installation was successful. This is _particularly important when using development versions from the [git source repository](Source.md)_ because these snapshots typically contain new features and code that is less well tested.

## Availability and installation ##

<ol>
<li>You can always download the latest release from the <a href='http://code.google.com/p/mdanalysis/downloads/list'>Download</a> page. Make sure to pick the <i>same release number as the version of MDAnalysis</i> that you have installed. Then unpack and install with the usual Python commands:<br>
<pre><code>curl -O MDAnalysisTests-0.7.5.tar.gz    # download from the commandline<br>
tar -zxvf MDAnalysisTests-0.7.5.tar.gz  # unpack<br>
cd MDAnalysisTests-0.7.5<br>
python setup.py install                 # (add options to install as needed...)<br>
</code></pre>
</li>
<li>Alternatively, use <code>easy_install -U</code> (or <code>pip install</code>) for a direct installation from web:<br>
<pre><code>easy_install -U http://mdanalysis.googlecode.com/files/MDAnalysisTests-0.7.5.tar.gz<br>
</code></pre>
Note that MDAnalysisTests contains MDAnalysis of the same release number as a dependency. This means that MDAnalysisTests 0.7.5 will try to automatically also install MDAnalysis 0.7.5.<br>
</li>
<li>Finally, the most convenient approach is to simply install <a href='http://pypi.python.org/pypi/MDAnalysisTests'>MDAnalysisTests from PyPI</a>:<br>
<pre><code>easy_install -U MDAnalysisTests<br>
</code></pre>
or if you prefer <code>pip</code>:<br>
<pre><code>pip install MDAnalysisTests<br>
</code></pre>
</li>
</ol>

## Running the tests ##
See UnitTests.


