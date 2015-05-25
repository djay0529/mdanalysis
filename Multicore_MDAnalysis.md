# Introduction #

Although MDAnalysis generally doesn't exploit multicore computer architectures under the hood (yet), it is possible to use the Python [multiprocessing](http://docs.python.org/2/library/multiprocessing.html) module to distribute MDAnalysis tasks over multiple cores. This is particularly useful if you have many replicate trajectories, but you can even use multiple cores to save a substantial amount of time when parsing a single coordinate file if the analysis is sufficiently demanding and/ or the system is sufficiently large. Hopefully this page will allow users to develop strategies to reduce analysis time and perhaps inspire developers to incorporate multicore support into appropriate components of MDAnalysis.

For the moment, this page is organized as a series of examples of the usage of MDAnalysis on multiple cores.

# Single coordinate file parsed with 32 cores #

Let's say you have a coordinate file with a bunch of DPPC residues in a vesicle (i.e., the standard MARTINI example here: http://md.chem.rug.nl/cgmartini/images/applications/vesicle/dppc_vesicle.gro ) and you want to assess the number of contacts around each individual residue within a certain cutoff distance. Although I will use this specific coordinate file in this example, the real power of the multiprocessing approach would become clearer with a much larger coordinate file with many thousands of DPPC residues.

This example takes advantage of the multiprocessing.Pool() functionality to produce a pool of worker processes equivalent in number to the available cores on the system and uses some custom-made code to distribute the distance analysis workload between them.

To start off with the hardest part, code a general purpose module that can assess the number of contacts within a certain cutoff distance of every individual residue of a given species:

```
'''
Author: Tyler Reddy
Purpose of module: Demonstrate utility of Python multiprocessing module for analysis of a single coordinate file with MDAnalysis. This module is also a useful general piece of code for analyzing a system that may have 'questionable contacts--' steric issues with atoms potentially too close together for any number of reasons (perhaps the most common use case would be after the construction of a complex system).
'''

#import some useful modules:
import MDAnalysis, multiprocessing, time, sys, numpy, scipy
import cPickle as pickle

#We normally parse coordinate files with MDAnalysis as Universe objects, but these contain open files and cannot easily be passed between cores, so instead each core will launch the same analysis function with a different set of arguments, load a copy of the universe object, perform the analysis on the subset of the coordinates specified in the arguments, and finally return the results to the parent process for aggregation with results from other cores

def perform_portion_of_steric_analysis_on_individual_core(coord_file, species_start_index, species_end_index, particles_per_residue,cutoff=2.0):
    '''Each core will individually load the universe object (as it cannot be passed between them) and produce distance array data for a subset of the index range of interest.
    The arguments:
    coord_file : is a string containing the path to the single coordinate file being parsed
    species_start_index : is the atom index for the first particle to be parsed by THIS core
    species_end_index : is the atom index for the last particle to be parsed by THIS core
    particles_per_residue : is the number of particles in a given residue (i.e., 13 for coarse-grained POPC in MARTINI) so that the code can stride through and assess contacts around a single residue at a time    cutoff : the farthest a particle can be from a given residue to be counted as a proximal contact (measured in A)
    *Note: this code assumes that the indices of a given species are contiguous in the coordinate file.'''
    #use the multiprocessing module to print the name of the current process, so that we can monitor progress at the terminal:
    print multiprocessing.current_process().name, 'Starting' 
    #as usual with MDAnalysis, we produce a Universe object in order to do most of our work, and this will be done on each core individually as there's no easy way to pass Universe objects (which contain open files) between cores:
    input_universe = MDAnalysis.Universe(coord_file) #again, this will happen on every core that runs this function
    #we'll want to count the number of atoms that are within some cutoff of a given residue, and store all the values calculated on this core in the following list:
    ordered_list_steric_violation_counts = []
    #now we stride through the range of indices for this particular residue (i.e, POPC or DPPC, etc.) and count the number of atoms / particles that fall within the specified cutoff:
    current_species_index = species_start_index
    while current_species_index < species_end_index:
        current_species_residue_start_index = current_species_index #first atom in current residue
        current_species_residue_end_index = current_species_index + (particles_per_residue - 1) #last atom in current residue
        atom_selection_within_cutoff_angstroms = input_universe.selectAtoms('around {cutoff} (bynum {start_index}:{end_index})'.format(start_index = current_species_residue_start_index, end_index = current_species_residue_end_index, cutoff = cutoff)) #simply select all atoms within cutoff A of the current residue
        number_of_atoms_within_cutoff = atom_selection_within_cutoff_angstroms.numberOfAtoms() #since my plan is to make a histogram, I'm really just interested in the number of violations for a given residue
        print multiprocessing.current_process().name, 'number of atoms within cutoff',number_of_atoms_within_cutoff #to monitor on a per-process basis at the terminal
        ordered_list_steric_violation_counts.append(number_of_atoms_within_cutoff) #store the steric violation counts for each residue in the overall list for this particular process
        current_species_index += particles_per_residue #stride forward to the starting index of the next residue
    print multiprocessing.current_process().name, 'Finishing' #to monitor end of this process on terminal
    return ordered_list_steric_violation_counts #remember, this list only covers the steric violations for the subset of residues parsed on a particular core

def adjust_arrays_to_avoid_splaying(list_index_arrays, particles_per_residue):
    '''Adjust the arrays of indices sent to each core to avoid the splaying of residues across cores. After other code roughly balances out the number of indices to be sent to each core, this function takes the list of index arrays destined for each core and performs an adjustment such that the returned list contains index arrays which only have whole residues--NO splaying of residues between arrays/cores can be tolerated for sensible results. Naturally, it would be much simpler to deal with residue numbers rather than indices in simple cases, but index numbers are often more reliable in extremely large systems or in other coordinate files where there may be non-unique residue numbers.'''
    current_element = 0
    for index_array in list_index_arrays: #each index_array is destined for a core (later in the code)
        while index_array.size % particles_per_residue != 0: #don't exit the while loop until you have an integer number of residues accounted for in the index array
            index_array = numpy.concatenate((index_array,numpy.reshape(index_array[-1] + 1,(1,)))) #add the next index value
            #each time I add a value to the end of the above array I'll have to remove the first element of the subsequent array to avoid duplicating an index in my overall analysis:
            list_index_arrays[current_element + 1] = list_index_arrays[current_element + 1][1:]
        #it might seem like the above could fail on the last element (last array) in the list, but by definition that array should always be divisible by the particles_per_residue when we get to it because the overall number of indices is also divisible by the number of particles in a residue, so the remaining indices (last element / array) should not require modification
        list_index_arrays[current_element] = index_array #assign the new array, which only contains whole residues, to the list of arrays
        current_element += 1 #increment the array I'm working on after the current array is divisible by the particles per residue
    return list_index_arrays

#I'm now going to write a main control function for the module, so that when it is imported for a specific-use case on any arbitrary number of cores, the code will attempt to gracefully distribute the workload over the various cores:

def main(start_index,end_index,coordinate_file, particles_per_residue, cutoff):
    '''Main control function of the process. The arguments are as described for the per-core function above, except that the start and end indices are the overall start and end indices whereas the previous function receives an index range which is a subset of the index range specified here (this housekeeping work is dealt with by the code in this function).'''
    #I need to split the topology indices into contiguous chunks of residues (NO partial residues) so that different index ranges can be parsed by different cores:
    available_cores = multiprocessing.cpu_count() #determine number of CPUs available
    #start with a single array containing the full range of indices:
    index_array = numpy.arange(start_index,end_index + 1) #see numpy documentation
    #produce a list of arrays to be passed to the cores, with each array as evenly balanced as possible in terms of the number of indices it contains (use numpy array_split):
    list_index_arrays_to_distribute_to_cores = numpy.array_split(index_array,available_cores)
    #**note that an even (or uneven) workload balance between cores does NOT guarantee that residues are not splayed across cores; for this, I'll use the function defined above--which will shuffle the indices assigned to each core until each core is assigned an index range that contains only WHOLE residues (more important priority than a precise load balance):
    list_index_arrays_to_distribute_to_cores = adjust_arrays_to_avoid_splaying(list_index_arrays = list_index_arrays_to_distribute_to_cores, particles_per_residue = particles_per_residue)
    #**want to be absolutely certain that each core receives a number of indices that is divisible by particles_per_residue:
    for index_array_for_core in list_index_arrays_to_distribute_to_cores:
        assert index_array_for_core.size % particles_per_residue == 0, "index arrays are splaying residues across cores"

    #finally, can start doing some of the multiprocessing heavy-lifting:
    pool = multiprocessing.Pool() #activate a pool of worker processes equal to the number of cores available on the system
    cumulative_parent_list_steric_violation_counts = [] #this is where ALL per-residue steric violation counts will end up (in the parent process, progressively incorporating results from child processes)
    #note that the order of the results from children -- > parent is not known ahead of time in this module, but we could tag the results from each child in a dictionary, etc., if we had a workflow that required this

    def log_result_pool(list_from_this_process): 
        '''This function will be called after each child process exits so that the overall list of steric violations in the parent is extended by the list of values produced in a given child.'''
        cumulative_parent_list_steric_violation_counts.extend(list_from_this_process)

    #now, iterate through the list of index arrays and hand the steric assessment tasks off to the various cores:
    for index_array in list_index_arrays_to_distribute_to_cores:
        starting_index = index_array[0]
        ending_index = index_array[-1]
        #the apply_async function calls the function we wrote earlier for use by individual cores, and specifies which indices to use on a given core; the callback argument allows us to use the above logging function to dump the data back to the parent for aggregation purposes
        pool.apply_async(perform_portion_of_steric_analysis_on_individual_core, args = (coordinate_file, starting_index,ending_index,particles_per_residue,cutoff),callback = log_result_pool)
    #the next two methods basically ensure that the parent process waits for all the child processes to complete (see the multiprocessing docs for details)
    pool.close()
    pool.join()
    return cumulative_parent_list_steric_violation_counts #so if you use this code within another module, you'll just get the overall list of steric violations per residue

def test_code():
    pass #place test code here eventually if you need to unit test

if __name__ == '__main__': #basically, only execute what follows if this module is executed as a top-level script (directly called on the command line rather than imported)
    test_code()
```

With the above general-purpose multicore steric assessment module (steric\_assessment\_general.py) written, we should be able to reuse the code for a variety of different files / residues. In this case, I'll write a short module to call the above module and plot steric assessment results for the DPPC coordinate file mentioned:

```
'''Author: Tyler Reddy
   Purpose: To perform a multicore steric assessment using a standard test file along with the multiprocessing-based general-purpose module built above.'''

#import some useful modules:
import MDAnalysis, time, numpy, scipy, sys
import cPickle as pickle
import steric_assessment_general #you may need to ensure that the module is in your path with sys.path.append('path/to/module') for example

start_time = time.time() #start time in seconds for benchmarking purposes

#for this test I'll parse the close contacts for DPPC in the vesicle coordinates available from the MARTINI website here: http://md.chem.rug.nl/cgmartini/index.php/downloads/example-applications/71-vesicles

#produce an overall list of DPPC steric violations using the general multicore code in another module and convert to numpy array:
array_DPPC_steric_violations = numpy.array(steric_assessment_general.main(start_index = 1,end_index = 10524,coordinate_file = 'dppc_vesicle.gro',particles_per_residue = 12, cutoff = 8.0)) #note that I've exaggerated the cutoff size a bit as the standard file doesn't likely have any major steric violations at, say, 2.0 A

#because this may take quite a while, pickle the array to be safe (see pickle docs for details):
pickle.dump(array_DPPC_steric_violations,open('DPPC_steric_viols.p','wb'))

#when simply adjusting the plot, can load directly from pickle with the above code commented (no need to re-run analysis code unless you want to change parameters):
#array_DPPC_steric_violations = pickle.load(open('DPPC_steric_viols.p','rb'))

#now plot the data (see matplotlib docs):
import matplotlib, matplotlib.pyplot

fig=matplotlib.pyplot.figure()
ax = fig.add_subplot(111)
matplotlib.pyplot.xticks(rotation=90)
histogram,bins = numpy.histogram(array_DPPC_steric_violations,bins=20)
bincenters = 0.5*(bins[1:]+bins[:-1]) #adjust to center them 
percent_denominator = array_DPPC_steric_violations.size / 100.0
ax.bar(bincenters,histogram / percent_denominator,facecolor = 'green',alpha=0.75,width=2) #percent histogram
ax.set_xlim(-1,95)
ax.set_xlabel('# of contacts within 8.0 $\AA$')
ax.set_ylabel('% of DPPC residues')
fig.set_size_inches(4,6)
fig.savefig('DPPC_steric_histogram.png',dpi=300)

print 'Steric assessment code completed in',time.time() - start_time, 'seconds'
```

The parallel code executed in 16.34 seconds using 32 cores on a single machine. The resulting steric assessment histogram (green) is shown below alongside the (identical) single core result (red). Hacking the code to operate on a single core (with a single universe object), the execution time is 212.69 seconds (13.01 X slower than in parallel). The amount of time saved could obviously increase substantially with a larger system, or if you were to apply this strategy to the frames of a given trajectory.

<img src='http://wiki.mdanalysis.googlecode.com/git/images/DPPC_steric_histogram.png' alt='Smiley face' width='400'> <img src='http://wiki.mdanalysis.googlecode.com/git/images/DPPC_steric_histogram_single_core.png' alt='Smiley face' width='400'>

<h1>Frame Counting Example</h1>

<pre><code>#author: Tyler Reddy<br>
#purpose of script: demonstrate utility of deploying MDAnalysis on multiple cores versus in serial<br>
<br>
import MDAnalysis, multiprocessing, numpy, sys, time, MDAnalysis.analysis.distances<br>
from MDAnalysis.tests.datafiles import PSF,DCD   # test trajectory<br>
<br>
#don't execute any code before if __name__ = '__main__' check because of the way multiprocessing imports code on other cores<br>
<br>
#define a function to check that the computer you are testing with has more than 1 core (otherwise no point!):<br>
def check_num_cores():<br>
    available_cores = multiprocessing.cpu_count()<br>
    if not available_cores &gt; 1: sys.exit('Need more than 1 core to exploit multi-core code.')<br>
    return available_cores<br>
<br>
#Now define a function to create as many Universe objects as you have cores for testing. Obviously, N duplicate copies of the same test trajectory is less useful than a real situation where you might have N parts of a really large trajectory or N replicates of a given simulation condition, but it's the same strategy in those cases:<br>
def create_universe_objects(available_cores):<br>
    list_universe_objects = []<br>
    for process_number in range(0,available_cores):<br>
        list_universe_objects.append(MDAnalysis.Universe(PSF,DCD))<br>
    return list_universe_objects<br>
<br>
#Define equivalent serial and parallel functions to perform a bunch of tasks and print out the frame number for tracking on terminal:<br>
#Note that parallel will only be faster if the functions do enough work to offset the overhead of starting parallel processes<br>
<br>
def serial_frame_counting(universe_object):<br>
    for ts in universe_object.trajectory[::]:<br>
        #do a bunch of random stuff:<br>
        COG = universe_object.selectAtoms('all').centerOfGeometry()<br>
        selection_1 = universe_object.selectAtoms('prop abs z &lt;= 5.0')<br>
        MDAnalysis.analysis.distances.self_distance_array(universe_object.selectAtoms('all').coordinates())<br>
        print 'frame :',ts.frame<br>
<br>
def parallel_frame_counting(universe_object):<br>
    print multiprocessing.current_process().name, 'Starting' #to monitor start of this process on terminal<br>
    for ts in universe_object.trajectory[::]:<br>
        #do the same random stuff as the serial version:<br>
        COG = universe_object.selectAtoms('all').centerOfGeometry()<br>
        selection_1 = universe_object.selectAtoms('prop abs z &lt;= 5.0')<br>
        MDAnalysis.analysis.distances.self_distance_array(universe_object.selectAtoms('all').coordinates())<br>
        print 'frame :',ts.frame<br>
    print multiprocessing.current_process().name, 'Finishing' #to monitor end of this process on terminal        <br>
<br>
def main():<br>
    '''Main control function.'''<br>
    #check the number of cores available:<br>
    available_cores = check_num_cores()<br>
    #create a list of universe objects as long as the number of cores available on your system:<br>
    list_universe_objects = create_universe_objects(available_cores)<br>
<br>
    #the serial test is trivial; I will just bound it with timestamps for benchmarking:<br>
    start_time = time.time()<br>
    for universe_object in list_universe_objects:<br>
        serial_frame_counting(universe_object)<br>
    end_time = time.time()<br>
    serial_run_time = end_time - start_time # in seconds<br>
<br>
    #the parallel test has a bit more going on, but can easily be adapted for many other applications:<br>
    start_time = time.time()<br>
    jobs = [] # for monitoring parallel jobs<br>
    for universe_object in list_universe_objects: #create a bunch of Process objects, which operate independently on different cores<br>
        process = multiprocessing.Process(target = parallel_frame_counting, args = (universe_object,)) #function args fed in as tuple for each core<br>
        jobs.append(process)<br>
        process.start() #self-explanatory; actually starts the child process<br>
    #now we don't want control flow to continue in the parent until all of the children are finished doing their work:<br>
    for job in jobs:<br>
        job.join() #join method will block indefinitely until the child process is done<br>
    #when control flow gets here in the parent all children will have completed<br>
    #at this stage you could feed a numpy array of data from each child back to the parent via a Queue or Pipe object in the multiprocessing module; I won't do that here, but you can easily build in automated plotting by combining these arrays and then using matplotlib in the parent, etc.<br>
    #important to note that interprocess communication requires that the object being sent can be pickled; so numpy arrays will work but Universe Objects in MDA, bounds methods, generators and file objects will not; so design your code around this    <br>
    end_time = time.time()<br>
    parallel_run_time = end_time - start_time # in seconds<br>
<br>
    print 'speed up: ', serial_run_time / parallel_run_time , 'X', 'for', available_cores, 'cores'<br>
    #I get: speed up:  6.87044755012 X for 12 cores<br>
    #speed up:  1.59204461414 X for 2 cores<br>
    #The case for parallelism is stronger if we make the per-trajectory analysis more complicated (slower)<br>
<br>
if __name__ == '__main__': #don't execute when importing in child processes<br>
    main()<br>
<br>
</code></pre>


<h1>Details</h1>

Add your content here.  Format your content with:<br>
<ul><li>Text in <b>bold</b> or <i>italic</i>
</li><li>Headings, paragraphs, and lists<br>
</li><li>Automatic links to other wiki pages