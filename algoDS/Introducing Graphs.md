# Introducing Graphs

# An Introduction to Graphs

## Graphs

We use graphs to represent actual entities, and the connections between them. Graphs allow us to reason about such connections.

## Directed Graphs

A directed graph is a set of nodes $V$ and edges $E$, where $E \subseteq V \times V$

Set of edges $E$ is a subset of cartesian product of vertices against itself ($V \times V)$. Each of the elements of the tuple of the edge, has to be a vertex (what?)

<aside>
💡 In mathematics, the Cartesian product of two sets A and B, denoted as A x B, is the set of all ordered pairs (a, b) where a is an element of A and b is an element of B.

</aside>

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled.png)

The graph is also made up of sets of edges, that can be seen as the arrows between the nodes

$V = \{1,2,...,9\}$

$E=\{(1,2),(1,3),(2,4),(2,5)... (9,2)\}$

The set of edges $E$ is made up of many ordered tuples

<aside>
💡 Tuple is a data structure similar to a list, but it is immutable, that is it’s elements once created can not be modified

</aside>

Each tuple, represents a connection between two vertices, and these are ordered because this is a **directed graph**

### What is meant by ordered tuples?

The first element in a tuple is the **from** and the second element is the **to.** For example, in a tuple $(a,a')$, $a$ is from where the node points to $a'$. In simpler terms, $a \rightarrow a'$. This is representing a connection

### Path

A path is a sequence of nodes, all of which are in the set $V$, the set of vertices. A path can also be thought of as traversal through the graph.

$Path:$ Sequence of nodes $u_1...\space u_n$, with $\forall i \in \{1... \space n-1\}$. $(u_i, u_{i+1})\in E$ e.g. 1,2,4,8

We have to make sure that there is an edge from one node to another along the path we are traversing. For example if we are traversing to node 8 from node 1, we need to make sure node 2 has a path from node 1, node 4 has a path from node 2 and node 8 has a path from node 4.

![Observe the darker arrows signifying a path from node 1 to node 8](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%201.png)

Observe the darker arrows signifying a path from node 1 to node 8

### Cycle

A path with the same start and end point. For example, 2, 4, 9, 2.

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%202.png)

### Simple Graph

A graph containing:

- No parallel edges (two edges with the same origin and destination)
- No self-loops (origin same as destination)

### Subgraph

A graph that is a subset of another graph. Can contain some of the edges or vertices.

### Spanning Subgraph

A subgraph that contains all nodes, but not necessarily all the edges

As seen from the diagram on the right, we have included all nodes (vertices) but we have only included some edges (bold arrows).

For a spanning subgraph, we do not need all nodes to be connected (look at node 9 in the picture to help you understand, i.e. no node directs (edges, or points to) towards node 9)

If we were to require all of the vertices to be connected, then that would have been a spanning tree**,** where every node is connected.

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%203.png)

### Operations

- `EMPTY` returns empty graph. $E \leftarrow \phi$
- `ADDEDGE(u,v)` adds an edge. $E \leftarrow E \space \cup \{ (u,v)\}$
- `REMOVEEDGE(u,v)` removes an edge. $E \leftarrow E \space \backslash  \{ (u,v)\}$
- `ISEDGE(u,v)` checks for edge. $(u,v)\in E$
- `SUCCS(u)` return successors of a node $\{ v \space|\space(u,v)\in E \}$

Time Complexities of mentioned Operations:

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%204.png)

### Adjacency Lists

Storing a list of successors for each node.

Successor: Node that has an edge going to it

Map each node to list of successors

- $E = \{ (u,v)\space|\space v \in succs(u) \}$
- directly implements $succs(u)$
- for integer nodes: use array of (dynamic) arrays

Memory $O(|V|\space+\space |E|)$, sum of number of vertices and number of edges

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%205.png)

$succs[1] = \{2,3 \}$

$succs[2] = \{4,5 \}$

$succs[3] = \{6,7 \}$

…

$succs[5] = \{ \}$

^ no node going towards any node from node 5

$succs[6] = \{5 \}$

### Adjacency Matrix

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%205.png)

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%206.png)

A two dimensional array of size number of vertices squared, filled with booleans. Look at the graph $(1,2)$ and on the table to find a $T$ between row = 1, column = 2.

For undirected graphs, the adjacency matrix will be symmetrical

Store $|V| \times |V|$ maps to Booleans

- $E = \{(u,v)\space | \space m(u,v) = TRUE \}$
- For integer nodes: use 2-dimensional array

Memory $O(|V|^2)$

- Bad for sparse graphs $(|E|\ll |V|^2)$. (what are sparse graphs?)

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%207.png)

### How to decide what to use?

- How dense is the graph?
- What operations would we be performing on it?

For example, large social media network best using adjacency list as millions of users (entries), not efficient in adjacency matrix

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%208.png)

# Graph Traversal Techniques

Graph traversal refers to the process of visiting nodes in a graph data structure, either breadth-first or depth-first, to visit every vertex and process its edges.

Look at the image below

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%209.png)

🙂 is where current pointer points at. We are currently at room (node) 1 and from room 1, we can explore nearby rooms which are 2, 3 and 4. Therefore, we have only explored room 1, and the picture to the left shows what other rooms we are yet to explore (which is also a list). 

Traversing through the rooms, for instance if we are at room 5 now, we have explored rooms 1,2,3 and 4. We have not explored room 6 which is what we have to explore after exploring room 5 first. After exploring room 6, To explore list becomes empty as we do not have any further rooms to explore, and explored list becomes (1,2,3,4,5,6), and we have explored all rooms.

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2010.png)

## Generic Graph Traversal Algorithm

We want to ensure that we find all nodes reachable in a graph, and to do this, we methodically visit edges, to new nodes until all nodes are discovered. In order to do this, we make two sets / lists:

- $D:$ discovered, but outgoing edges yet to be explored
- $F:$ finished, outgoing edges have been explored

Pseudocode implementation for a procedure $EXPLORE(S)$

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2011.png)

On each iteration, we select a node and search for any new reachable nodes and this is going to continue, and this is going to continue until we do not have any more nodes left to explore.

- Example that goes through the above procedure
    
    Start at node 1, put that in D set (discovered) and find it’s outgoing nodes. Once found, then mark 1 as finished (blue).
    
    ![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2012.png)
    
    Now that we know 1 had {2,3} as outgoing edges, mark those as discovered and 1 as finished
    
    ![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2013.png)
    
    Pick any from {2,3} and explore them. For example, we decide to explore the children of node 2, we get {4,5}. So together, we have {3,4,5} to discover (explore) and we mark 2 as finished.
    
    ![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2014.png)
    
    We keep going on till there are no more nodes to be discovered, also if a discovered node does not have any outgoing edge, that node is going to be a finished node, for example node 5 in this case.
    
    ![That’s his forehead fr](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2015.png)
    
    That’s his forehead fr
    
    How the graph would look like after every node has been discovered
    

### Generic Graph Traversal (Correctness)

$EXPLORE(S)$ returns exactly the nodes reachable from $S$

- Any node in $D \space \cup \space F$ is reachable:
    - Initially, only $S \in D \space \cup \space F$
    - Only successors of nodes in $D \space \cup \space F$ added
- If a node is reachable, it will be included in $F$:
    - During the loop, successors of finished nodes are finished or discovered
    - Finally, $D = \phi$, thus successors of finished nodes are finished

$\rightarrow$ Every reachable node is finished (follow path from $S$)

## Depth First Search (DFS) vs Breadth First Search (BFS)

In what order do we process nodes from $D$

![Swapped the two last lines in the while loop?](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2016.png)

Swapped the two last lines in the while loop?

### Depth First Search

Last in / first out (stack): Depth First Search (DFS)

<aside>
💡 Recall how a stack works

</aside>

1 is in the stack which we are going to discover. {1}

After one is explored, it is popped out of the stack and 3 and 2 are added, as 2 is added in the end it would be the first one to be discovered and popped {3,2}

2 is popped off and 2’s edges 5 and 4 are added. Now 4 would be discovered. {3,5,4}

New stack is {3,5,8,9} after 4 is popped off. Pretty self-explanatory no? 

Once 8 and 9 are discovered and popped off, as they do not have edge notices, they are just added to the finished list and we can go on to then discover 5 and 3 (this order).

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2017.png)

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2018.png)

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2019.png)

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2020.png)

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2021.png)

### Implementing a DFS

DFS has a nice recursive implementation, you go deep down a node until you backtrack to the other node and go deeper into that.

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2022.png)

### Breadth First Search

First in / first out (queue): Breadth First Search (queue)

So you discover the root node (1) to get {2,3}. Then, you discover on {2} to get {4,5}, however since this is breadth first search and implements a queue, we add {4,5} to the end of the queue. The resulting queue would look like {3,4,5} and you discover 3 before moving onto the search.

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2023.png)

# Topological Sorting

A topological sort is applicable when we have a set of tasks, with particular dependencies. These dependencies must be met before executing the tasks

A set of tasks, for example:

- A build sequence when developing software
- Prerequisites between courses of a degree
- Scheduling task constraints in a project

Dependencies $\rightarrow$ tasks that needs to be completed before a task can be started

Model as directed graph:

- Edge$(u,v):v$ depends on $u$.
    
    What does this mean
    
    You must complete task $u$ before you can proceed with task $v$ as task $v$ depends on task $u$
    

Find a build sequence

- Example of Topological Sorting
    
    ![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2024.png)
    

### Steps?

- Arrange nodes sequentially, such that all edges point forwards
    - Intuition: All prerequisites come earlier in sequence
- Topological sorting possible iff graph has no cycles
    - Directed Acyclic Graph (DAG) 😩
        
        DAG: Edges are directed and no cycles exist
        

<aside>
💡 Recall what a cycle was

</aside>

- Now: Can use DFS based algorithm for topological sorting and cycle detection

### Cycle Detection with Depth First Search (DFS)

- When first encountering a node, mark it as O or Open O(pen)
- Only when finished exploring it’s children, mark it as D or Done D(one)
- N or New N(ew)
- Whenever we encounter an open node again, it’s a cycle
- When node is done, all it’s successors are already done
- Node done in reverse topological order
- To get topological sort: *prepend* nodes to list when marked as done
    
    <aside>
    💡 Prepend: Add something to the beginning of a list
    
    </aside>
    

How do we know if there exists a cycle?

If we traverse deeper and deeper and find a node that is already open, this means we have a cycle in out graph.

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2025.png)

<aside>
💡 Reverse topological order is the reverse of a topological sort, which is a linear ordering of the vertices of a directed acyclic graph such that for every directed edge (u,v), vertex u comes before v in the ordering. In reverse topological order, the vertices are ordered in such a way that for every directed edge (u,v), vertex v comes before u.

</aside>

 

### Topological Sorting Example 1

![topoSortingEx1.gif](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/topoSortingEx1.gif)

From how I understand, whenever you got to backtrack from a node, i.e. the node you are backtracking from has no edges directed to another node, that’s when you mark that node as Done and you backtrack till you find a similar node(by similar node I mean a node with no directed edge going out of it).

### Topological Sorting Example 2

![Untitled](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/Untitled%2026.png)

Note that there is a cycle between 6,3 and 5

![topoSorEx2.gif](Introducing%20Graphs%2014a72d54778a46e2ba4e8fabc6179310/topoSorEx2.gif)

Therefore, from 6 we go to 3 and at 3 we can find out that there is a cycle back to 6 which was an OPEN node and hence we get an error that stops the algorithm.