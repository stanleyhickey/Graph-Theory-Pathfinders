# Graph-Theory-Pathfinders
## Classifying Eulerian and Hamiltonian graphs and implementing algorithms to identify and trace their characteristic circuit paths.


In 1736, Leonhard Euler showed that there was no way to travel across the Seven Bridges of Königsberg, crossing each bridge a single time and ending up where you started. He did this by showing that there is no *Eulerian circuit* in a graph representing the city, where vertices represent landmasses and edges represent bridges between them. This established the first result in graph theory.

More precisely, an *Eulerian circuit* is a cycle of vertices v_1, v_2, ..., v_k = v_1 such that every edge of the graph appears as (v_i, v_{i+1}) and no edge is repeated. In other words, it is a cycle which contains every edge of the graph precisely once. You can think about this as some way of travelling around the graph, crossing each edge precisely once. Note that this does **not** mean that each vertex has to be visited precisely once (or even at all!)

We say that a graph is *Eulerian* if there is some Eulerian circuit in the graph.

Euler's realisation was that the existence of an Eulerian circuit is related to the degrees of the vertices.

## Theorem: a graph has an Eulerian circuit if and only if every vertex has even degree, and all of the vertices of positive degree are in one connected component.

A few examples are shown in graphs.png. 

1: This example is Eulerian: every vertex has even degree, and there is only one connected component. An Eulerian circuit is [0, 1, 2, 3, 4, 1, 3, 0].

2: This example is not Eulerian: it has a single connected component, but vertices 1 and 4 have odd degree.

3: This example is Eulerian: it does have multiple connected components, but only one of them contains vertices of positive degree, and each vertex has even degree. An Eulerian circuit is [0, 1, 2, 3, 4, 1, 3, 0].

4: This example is not Eulerian; every vertex has even degree but the positive-degree vertices don't lie in a single connected component.
