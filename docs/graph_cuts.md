# Making Graph Cuts Based on Statistical Analysis
For some graphs, it is very useful to make cuts based on certain statistical characteristics.  Intelligently pruning 
noise is our goal; relatively inconsequential data that obfuscates the key understanding of a specific graph is often
found on the margins of the following concepts.
- Betweenness Centrality
- Degree Centrality
- Edge Weight

## Histograms and Cuts
All cuts are primarily predicated on first generating individual histograms of each measure.  After these histograms
are created, the user can decide on appropriate cut thresholds to use.  One technique to determine the efficacy of 
your cuts is to compare the graph's "Q-score" (`topologic.q_score()`) both before and after the cuts are made.

If the Q score moves away from ~ 0.7 in either direction, you may be making either too many cuts or too few. 

## Betweenness Centrality
[Betweenness Centrality](https://en.wikipedia.org/wiki/Betweenness_centrality) is a measure of how many times a vertex
finds itself on the shortest path between all the other vertices in a graph.  This calculation is actually quite 
expensive, so you are cautioned against using this for your first cuts of very large graphs - while we do
ultimately use *Brandes* algorithm, our Big-O is roughly O(V<sup>2</sup>logV+V*E), where V is the count of 
vertices and E of edges.

## Degree Centrality
[Degree Centrality](https://en.wikipedia.org/wiki/Centrality#Degree_centrality) is a measure of how many connections a
vertex has to other connections.  This calculation is much more manageable, and so any cuts made to a graph should 
be either Degree Centrality or Edge Weight.


## Edge Weight
Edge weight histograms are simply generated by creating a histogram over all the edge weights in a graph.  Making cuts
based on this histogram are reasonable from both directions - trimming off the smallest edge weights may result in 
culling large parts of the graph, but those parts may also be unimportant or even at odds of trying to understand a
specific characteristic of the graph.  Likewise, edges with weights far outstripping the median may inappropriately 
dominate the graph when determining layout or many other analayses.

## Making the Cuts
Each cut function takes in at least three important values: 
- The graph to make cuts upon.  Note that in **every case**, the graph is copied and pruned - it is never modified in place.
- A cut threshold - some singular value at which the cut algorithms decide whether to keep or remove a node or edge
- A cut process - an enumeration that determines whether the values to be cut should be greater than, greater than or 
equal to, less than, or less than or equal to the cut threshold.

See the api documents for `topologic.statistics` package for more specific details on these.