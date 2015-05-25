Until the beginning of 2014 we could get data from Google Code for how often MDAnalysis packages were downloaded (as described in this page) but since then Google stopped hosting release packages for download (see [Issue 155](https://code.google.com/p/mdanalysis/issues/detail?id=155)). We are now distributing release packages via the Python package index at https://pypi.python.org/pypi/MDAnalysis but we haven't set up a way to visualize the download data from there (see [Issue 167](https://code.google.com/p/mdanalysis/issues/detail?id=167)). In the meantime, a snapshot of the download numbers from Feb 19, 2015 yielded:

| **Release**                  | **Release Date**        | **Downloads** |
|:-----------------------------|:------------------------|:--------------|
|   MDAnalysis-0.7.6.tar.gz    |  2012-07-04             |     2,490     |
|MDAnalysis-0.8.0rc2.tar.gz    |  2013-12-28             |     1,235     |
|MDAnalysis-0.8.0rc4.tar.gz    |  2014-01-01             |     1,280     |
|   MDAnalysis-0.8.0.tar.gz    |  2014-01-20             |     1,475     |
|MDAnalysis-0.8.1rc1.tar.gz    |  2014-02-11             |     1,338     |
|   **MDAnalysis-0.8.1.tar.gz**  |  2014-04-01             |     **5,458** |

The download data were obtained with [vanity](https://pypi.python.org/pypi/vanity)
```
vanity MDAnalysis
```

# Historical download data #
We parse the [Downloads](http://code.google.com/p/mdanalysis/downloads/list) to generate statistics of how often the released versions of MDAnalysis have been downloaded. The graph contains the library itself (red), the UnitTests package (yellow) and also pre-releases (gray). Each bar corresponds to the total number of downloads until the date stated in the title of the graph.

![http://wiki.mdanalysis.googlecode.com/git/images/stats/MDA_downloads.svg](http://wiki.mdanalysis.googlecode.com/git/images/stats/MDA_downloads.svg)

This graph does not track unique downloads (i.e. a single person downloading multiple times is counted multiple times) and it also does not account for people using the git version directly from the source code repository.


# Technical details #

The "Downloads" tab contains a number of useful information (http://code.google.com/p/mdanalysis/downloads/list). Download counts for the increasing versions of MDAnalysis are particularly interesting. Unfortunately the page is in HTML and there is no way to fetch this information in a more friendly format, for example as a CSV file.

The code below fetches the Download's page, parses it and extracts the information by putting it a numpy record array (numpy.rec). Plotting is then done using matplotlib.

## Usage ##
In the images/stats directory of the git wiki repository, run
```
fetch_download_stats.py
plot_download_stats.py
```
This will scrape the statistics from the download page and prepare a
rough plot. Add to git and push: this website will then use the latest graph directly from the git wiki repository.

## Scripts ##
### fetch\_download\_stats.py ###
Fetch and parse (by Jan Domanski):
[fetch\_download\_stats.py](http://wiki.mdanalysis.googlecode.com/git/scripts/fetch_download_stats.py)
```
#!/usr/bin/env python
# Author: Jan Domanski
# see http://code.google.com/p/mdanalysis/wiki/DownloadStatistics

import mechanize
from bs4 import BeautifulSoup
import numpy as np

datafilename = "fetch_MDAnalysis_download_statistics.pkl"

br = mechanize.Browser()
url = "https://code.google.com/p/mdanalysis/downloads/list?can=1&q=&colspec=Filename+Summary+Uploaded+Size+DownloadCount"
resp = br.open(url)
html_doc = resp.read()

soup = BeautifulSoup(html_doc)
table = soup.find(id="resultstable")
rows = []
for tr in table.find_all("tr"):
    row = [td.string if not len(td.find_all('a')) else (td.find_all('a')[0].string) for td in tr.find_all("td") ]
    if not len(row): continue
    row = row[1:-1]
    row = [td.replace("\n","").strip() for td in row]
    row[-1] = int(row[-1])
    rows.append(tuple(row))
records_array = np.rec.fromrecords(rows, names=["Filename","Summary","Uploaded","Size","DownloadCount"])

import pickle
pickle.dump(records_array, open(datafilename, "w"))


```

### plot\_download\_stats.py ###
Plotting code (by Oliver Beckstein): [plot\_download\_stats.py](http://wiki.mdanalysis.googlecode.com/git/scripts/plot_download_stats.py)
```
#!/usr/bin/env python
# Author: Oliver Beckstein
# Year: 2012
# Licence: Released into the Public Domain
# see http://code.google.com/p/mdanalysis/wiki/DownloadStatistics

"""Simple script to process the download statistics for MDAnalysis

1. run Jan's script::

     fetch_download_stats.py

2. run this script::

     plot_download_stats.py

3. Look at the figure downloads.pdf

See  http://code.google.com/p/mdanalysis/wiki/DownloadStatistics.

"""

import numpy as np
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
import time, datetime
import re
import pickle
try:
    from collections import OrderedDict
except ImportError:
    try:
        from odict import odict as OrderedDict
    except ImportError:
        raise ImportError("need collections.OrderedDict or odict.odict")

# how distinguish and plot different release types
properties  = {
    'release': {
        'cre': re.compile(r"MDAnalysis-(?P<release>[0-9.]+)\.tar\.gz"),
        'color': 'red',
        'alpha': 1,
        },
    'tests': {
        'cre': re.compile(r"MDAnalysis(Tests|TestData)-(?P<release>[0-9.]+)\.tar\.gz"),
        'color': 'yellow',
        'alpha': 0.5,
        },
    'prerelease': {
        'cre': re.compile(r"MDAnalysis-(?P<release>[0-9.]+)-(?P<prerel>rc[0-9]+)\.tar\.gz"),
        'color': 'gray',
        'alpha': 0.3,
        },
    }

datafilename = "fetch_MDAnalysis_download_statistics.pkl"


data = pickle.load(open(datafilename))

# make date field canonical "Month Year" (Google displays the ones younger than ~6 months as "Month Day")
this_year = time.localtime().tm_year
for rec in data:
    month, day_or_year = rec.Uploaded.split()
    day_or_year = int(day_or_year)
    month = time.strptime(month, "%b").tm_mon
    if day_or_year <= 31:
        # change 'Month Day' to 'Month ThisYear'
        # should be ThisYear - 1 if longer than 6 months ...
        now = datetime.datetime.now()
        then = datetime.datetime(this_year, month, day_or_year)
        tdelta = now - then
        if tdelta.days < 0:
            # last year...
            #print("DEBUG: %s is last year" % time.strftime("%b %Y", (this_year, month, day_or_year, 0, 0, 0, 0, 0, 0)))
            then = datetime.datetime(this_year - 1, month, day_or_year)
            assert (now - then).days >= 0

            this_year -= 1
        rec.Uploaded = time.strftime("%b %Y", (this_year, month, day_or_year, 0, 0, 0, 0, 0, 0))

# conversion to plottable datetime
cvt = mdates.strpdate2num('%b %Y')
dates = np.array(mdates.num2date([cvt(d) for d in data.Uploaded]))
today_date = datetime.datetime.now()   # for max of x-axis

plotdata = OrderedDict((reltype, {'idx': [], 'date': None, 'downloads': None, 'releases':[]})
                       for reltype in properties)

# find indices into data for different types of releases
for idx, rec in enumerate(data):
    release_type = None
    for reltype, p in properties.iteritems():
        m = p['cre'].match(rec.Filename)
        if m:
            release_type = reltype
            break
    if not release_type:
        print "DEBUG: no release type identified for %r", rec
        continue
    plotdata[release_type]['idx'].append(idx)
    plotdata[release_type]['releases'].append(m.group('release'))

# extract the dates and downloads for each release
for d in plotdata.values():
    idx = d['idx']
    d['date'] = dates[idx]
    d['downloads'] = data.DownloadCount[idx]
    d['releases'] = np.array(d['releases'])

# plot each release
fig = plt.figure(figsize=(5,5))
fig.subplots_adjust(bottom=0.2)

def textlabel(rects, labels, offset=10):
    # http://matplotlib.org/examples/api/barchart_demo.html
    # attach some text labels
    for rect, label in zip(rects, labels):
        height = rect.get_height()
        ax.text(rect.get_x()+rect.get_width()/2., height+offset, label,
                ha='center', va='bottom', rotation=90)

ax = fig.add_subplot(111)
bars = {}
for release_type, d in plotdata.iteritems():
    p = properties[release_type]
    bars[release_type] = ax.bar(d['date'], d['downloads'], width=50, color=p['color'], alpha=p['alpha'], label=release_type)

y_min, y_max = ax.yaxis.get_data_interval()
l_y = y_max - y_min
offset = l_y/50.

textlabel(bars['release'], plotdata['release']['releases'], offset=offset)
#textlabel(bars['prerelease'], plotdata['prerelease']['releases'], offset=offset)

ax.xaxis_date()
ax.set_xlabel("release date")
ax.set_ylabel("total downloads")
ax.set_ylim(0, y_max + 160)  # heuristic, should measure length of last label...
ax.set_xlim(None, today_date)
plt.setp(ax.xaxis.get_majorticklabels(), rotation=45)
ax.legend(loc="best")
ax.set_title("MDAnalysis downloads (tar.gz) until %s" % time.strftime("%Y-%m-%d", time.localtime()))

ax.figure.savefig("MDA_downloads.pdf")
ax.figure.savefig("MDA_downloads.svg")
ax.figure.savefig("MDA_downloads.png")

```