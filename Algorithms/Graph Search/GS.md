# Graph search

Algorithms for traversing/searching a graph data structure.

## Breadth-First Search

It starts at some arbitrary node of the graph and explores all the neighbour nodes at the present depth before moving on to the nodes at the next depth i.e. it's neighbours.

## Depth-First Search

Starts at some arbitrary node and explores as far as possible along each branch before backtracking.

### Topological Sort

A topological sort/ordering of a directed graph $G$ is a labelling $f$ of $G$'s nodes such that:

- The $f(v)$'s are the set $\{1,2,...,n\}$
- $(u,v) \in G \Rightarrow f(u) < f(v)$

**Note**: Every directed acyclic graph has a sink vertex(vertex with no outgoing arc's in topological ordering)

#### Topological Sort via DFS

```
function DFS(graph G, start_vertex s)
```
