# MAPREDUCE
-    fault tolerant system(deals with all the faults) running over many workstations and scales to huge computations and datasets by running over many workshops by parallelization
-    logic for specific type of computations:
    -     Map: process input into (key,value) pairs and thats how they group things, *its only use if you have that kind of data and many apps use this pattern*
    -     sort: how they group things
    -     reduce: group the results and process each group             


1) parallelism needs concurrency
2) with mapreduce, concurrency is not our headache in parallelism
3) map and reduce can happen together

         
## why is sequential word count in python slow and whats wrong?
- the web is large and this is not parallel, so its gonna be a huge amount of time
- requires O(n) memory for n unique words, huge memory for mapreducing the entire web

# Easy
in the MR program, all the user needs to care about is the map and reduce function, the concurrency and parallelism is handled by machine

## MR abstraction:
read every line
split line into pieces
word 1 occurs 1 time
word 2 occurs 1 time
word 2 occurs 1 time
word 3 occurs 1 time

all in k-v pairs and then reduce takes all the k-v and totals them up and returns final k-v pairs

but as users, sometimes you have to think about what is the exact "key-value" that will give me the correct output that I want.

# word count ex:
- shuffle phase for gathering k-v from different workers


## why sorting in shuffle phase?
- sorting is for grouping and why do we sort?how can it address memory issue which hashing was not doing?the problem with hashing was in memory, what if our data is larger than the memory.  
    -   group different k-vs
    -   sorting can be done **offline and can run out of memory** and efficient algos could take over without memory spill over, **and hence it can scale tp big size/ability** to sort offline is an old-school nice method 
    -   nlogn time complexity

# what the penalty of shuffling?
- sorting is super expensive
- move data around across machines [IMP]

# What problems might occur:
 ## correctness:
-   some machine fails
-   save checkpoints in each mapper
-   and if o/p is lost, just recompute it like [Fox]
-   
 ## performance probs
 - if one mapper is really slow, it hurts overall performance
 -  unbalanced workloads due to hot keys and unbalanced inputs
 - 

# PERFORMANCE RESULTS FROM PAPER:
1) straggler problem leading to performance drawbacks

2) network BW shuffle phase to group and combine and give result

3) a "hot key" might way too much work to do: "the" occurs way to many times than any other word, so "the" shuffler has too much work to do

# Fault Tolerance:

# Performance from Paper:
## how much traffic do they move for mapping-shuffle-reduce in **normal execution** ?
- big peak in the beginning when every input is broken down to map, the amount of work it takes to do this phase
- in the shuffle phase, there are 2 bursts of peaks in the plot, the first peak takes place after the mapping phase and its sending the data everywhere it needs to go and then it falls off due to the sorting period ongoing along with the reducers getting ready and then the  second burst happens wherein more traffic is send to mapreduce again. 
- in the reduce phase, the reducers are spread out and the process goes on a bit longer

## the middle column talks about BACKUP TASKS which are stragglers:
but in the first column where normal exexutions are being performed, MR does straggler elimination, where towards the end of the Reduce process, MR says who are the ones who are late or slow, lets just start those workers again.

## whats the difference bw *with backup* and *without backup* tasks?
- in the middle column, tasks take longer to finish if we look at the x-axis. Is it doing more work? NO, its just that somebody is running slowly.
-  with backup actually, you do more work because you re-do the stragglers, but without backup/stragglers theres a long tail in the middle column last plot where the stragglers go on for long without a lot of useful work
- the final column is where they *kill some processes*, killing machine shows up as negative work, and its followed by positive work as the work starts back again.

# Batch to Streaming
