The **LeafletFinder** algorithm solves the problem of identifying the groups of lipids that make up the two leaflets of a membrane, given an AtomGroup of the membrane. It is implemented in [MDAnalysis.analysis.leaflet](http://pythonhosted.org//MDAnalysis/documentation_pages/analysis/leaflet.html), which also has examples.

The algorithm is described in the MDAnalysis paper ([PMID 21500218](http://www.ncbi.nlm.nih.gov/pubmed/21500218))  under [Implementation of the LeafletFinder algorithm for lipid bilayer analysis](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3144279/#S9). Briefly:

  1. Find the nearest neighbors of marker groups (such as the phosphorous atoms) within a chosen cutoff distance and construct a graph of all connected markers.
  1. Identify the largest connected subgraphs.
  1. Sort the subgraphs by size. The first and second largest ones are the membrane leaflets.

The cutoff distance needs to be less than the bilayer thickness and comparable to typical lipid-lipid in-plane distances. The algorithm works for membranes of any curvature, including vesicles.