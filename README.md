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

## Part 1

One of the proofs of the theorem above is constructive; it defines an algorithm and then shows that the algorithm outputs an Eulerian circuit given the hypotheses of the theorem. The algorithm is as follows:

```
1. Initialise a stack of vertices: pick a vertex u in G, and set stack = [u]
2. Initialise an output circuit: set circuit = []
3. While the stack is not empty:
    4. Call the last element in the stack current_vertex.
    5. If there are any edges from current_vertex that haven't already been considered:
        6: Pick one such edge {current_vertex, neighbour}.
        7. Note that this edge cannot be used again (from either direction).
        8. Append neighbour to the stack.
    9. Otherwise, if there are no such edges, then pop current_vertex from the stack and append it to circuit.
10. Return circuit.
```
The algorithm above finds an Eulerian circuit in a *connected* graph if it has one. In part 1 of this project I will implement this algorithm as a function eulerian_circuit(). Initially I will assume that the input graph is connected and Eulerian.

It is worth noting that I make use of the 'networkx' package when creating graphs.


## Part 1 Code Description:

This function follows the algorithm for finding an Eulerian circuit in a graph G, if one exists. While assuming that the input graph is a connected, undirected graph the function shows that a graph has an Eulerian circuit if and only if every vertex has even degree by ouputting a valid Eulerian circuit for every graph for which this is true.

The algorithm begins by checking if the degree of every vertex in the graph is even. If not, it returns None, as an Eulerian circuit is not possible in this case.

If all vertices have even degree, it chooses an arbitrary starting vertex u and initialises a stack with this vertex. It also initialises an empty list called circuit to store the vertices of the Eulerian circuit. It creates a set called visited_edges to store the edges that have been traversed once.

The algorithm then enters a loop that continues until the stack is empty. In each iteration of the loop, it looks at the last vertex on the stack, current_vertex, and finds its adjacent edges using the edges() method of the graph G. It loops through these edges and checks if an edge has not been visited yet. If it finds such an edge, it adds its adjacent vertex to the stack and adds the edge to the visited_edges set. It then breaks out of the inner loop and continues with the outer loop.

If it cannot find an unvisited edge from the current_vertex, it pops the current_vertex from the stack and appends it to the circuit list.

Once the loop terminates, it returns the circuit list, which contains the vertices of the Eulerian circuit.


## Part 2

In order to apply the theorem, we need to be able to check whether all of the positive-degree vertices are connected. One way of doing this is to start a traversal from any positive-degree vertex, and check if it visits all of the others. 

In this part I will implement a function `all_positive_degree_vertices_connected` which takes a `networkx` `Graph` and returns `True` if all vertices of positive degree are connected, and `False` otherwise using a breath-first traversal method.

## Part 2 Code Description:

This code checks whether all vertices in an undirected graph G with positive degree are connected to each other. If they are, it returns `True`, and if not, it returns `False`.

The first function, performs a breadth-first search traversal of the graph G starting from the vertex u. It initialises a queue with the starting vertex u, and a set seen to keep track of the vertices that have been visited. It then enters a loop that continues until the queue is empty. In each iteration, it dequeues the first vertex in the queue, current_vertex, and checks if it has already been visited. If it has not been visited, it adds it to the seen set and adds all of its neighbors to the queue. The function returns the number of vertices that have been added to the seen set.

The second function initialises an empty list called positive_degree_v to store the vertices with positive degree in the graph G. It then loops through all vertices in G, and for each vertex with degree greater than zero, it adds it to the positive_degree_v list.

It then chooses an arbitrary vertex u from the positive_degree_v list and performs a breadth-first search traversal of the graph G starting from u using the breadth_first_traversal() function. It compares the number of vertices visited during the traversal to the length of the positive_degree_v list. If they are equal, it returns True, indicating that all vertices with positive degree are connected to each other. Otherwise, it returns False.

## Part 3 

In the same way that an *Eulerian* cycle is one which contains every edge precisely once, a *Hamiltonian* cycle is one which contains every *vertex* precisely once (apart from whichever vertex is picked to be "first", which must be visited twice to obtain a cycle). Determinining whether a graph is *Hamiltonian* (contains a Hamiltonian cycle) is significantly harder than determining whether it is Eulerian. In particular, it is NP-complete.

In this part I will implement a function `hamiltonian_cycle` which uses a backtrack search to find a Hamiltonian cycle in a graph, if one exists. Subsequently, I will use this function to find a Hamiltonian cycle in the Hoffman-Singleton graph; a highly symmetric, undirected graph with 50 vertices and 175 edges. It is a regular graph, meaning each vertex has the same number of edges, specifically 7 edges per vertex. An image of a Hoffman-Singleton graph can be seen in HF.png

## General Backtrack Search Algorithm


1. Initialise any data you need to keep track of, such as your partial solution.
2. Define an inner function called "dive", which does the following:
3. If the partial solution is a potential full solution, then check that it is genuinely a solution.
   If it is, then return the solution. If not, then return None to represent a failure.
4. For each possible way of extending the partial solution:
5. Extend the partial solution in that way.
6. If this partial solution is consistent, then:
7. Call dive() to recursively search for a solution.
8. If dive() returned a solution, then return that solution immediately to "pass it up".
9. If this point is reached, then no solution was found extending this partial solution, so return None.
10. Return dive().

This is what I followed when writing the hamiltonian circuit function.

## Part 3 Code Description:

The hamiltonian_cycle function starts at vertex 0 and initialises the cycle with that vertex. It also creates a set to keep track of visited vertices. Then, it defines a nested function called dive() that performs the recursive search for the cycle.

The dive() function first checks if the potential solution (i.e 'cycle') contains all the vertices as well as if there is an edge between the first and last vertex. If so, it appends the starting vertex to complete the cycle and returns it.

If the cycle is not yet complete, the function gets the last vertex in the cycle and adds it to the set of visited vertices. Then, for each neighbour of the current vertex that has not been visited, the function adds the neighbour to the cycle and recursively calls itself. If a cycle is found, it returns the cycle. Otherwise, it removes the neighbour from the cycle and continues the loop. If no cycle is found the functiomn returns None.

Finally, I created a set of edges that belong to the hamiltonian cycle for the Hoffman-Singleton graph and coloured those edges in red while making the rest of the edges transparent to visualise the hamiltonian cycle of the graph next to its entire drawing.
