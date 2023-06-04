# Shortest Paths

# Shortest Paths with BFS

Shortest paths with Breadth first search

Task: We need to travel from Berlin → Munich with the following condition:

- Fewest possible train changes

Possible routes:

1. Berlin → Magdeburg → Chemnitz → Munich (3 trains)
2. Berlin → Bonn → Munich (2 trains)

Hence we would choose the second route to reach Munich on 2 trains - which may not necessarily be the faster route

![Directed graph, where the cities are nodes and the edges are the trainlines](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled.png)

Directed graph, where the cities are nodes and the edges are the trainlines

## Shortest Path

A shortest path between two nodes is the path that connects the two of them with the minimal number of edges

Shortest = minimal number of edges

Breadth First Search (BFS) visits nodes in order of their distance from our start node $S$

- Example of BFS
    
    An example of BFS could be visiting all destinations from Berlin that are 1 train away first, then visiting all destinations that are 2 trains away
    

Obtain path via the *****predecessor***** map:

- For each node, store node from which it was discovered
- Follow these nodes backwards, until $S$ is reached
- Convention: predecessor of $S$ is $S$ itself

### Procedure Shortest Path

Going to use a queue, which is a FIFO (first in first out) data structure

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%201.png)

$\pi$ is the predecessor

$u \leftarrow Dequeue(D)$: Return $u$, the element formerly at the front of D, and remove $u$ from $D$ (remove the element at the front like a typical queue)

$Enqueue(D,v):$ add $v$ to the end of $D$

What this algorithm basically does is: 

While $S$ is an element, in the set D, we remove the element from the top of the queue, and we place that into the finished set. Then we add all of the undiscovered successor nodes to the back of the queue. Simultaneously, we are going to add an entry to our predecessor map. That is going to tell us that these successes were discovered from this particular node that we removed from the set of the discovered nodes. To finish off, we are going to return the predecessor map $\pi$

### Procedure Get Path

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%202.png)

For each node $v$, *******predecessor map $\pi(v)$* stores the node from which $v$ was discovered

Obtain shortest path from $S$  via $\pi$

What this algorithm basically does is:

Gets our predecessor map $\pi$ and node $u$ and it returns to us the shortest path between $S$ and $u$, in simpler words we get our path via a backtracking method (getting our path in a reverse order)

So we are constantly adding predecessors to the front of the path until we reach the starting point again

### Example

Watch the gif carefully, first we add nodes to the empty queue, and we dequeue them, when we dequeue them, we add the children of that node and mark the previous node as finished and new ones as discovered, we keep going on till we have discovered every node.

Once done with discovering every node, we can now backtrack from the node we’re trying to find the shortest distance to, and we backtrack till our starting node, this path is our shortest path.

![GIF](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/ezgif-5-b91175d824.gif)

GIF

# Single Source Shortest Paths in Weighted Graphs with Bellman Ford Algorithm

Now, we want to look at the journey that takes the least amount of time to get to our destination. The edges on this map represent a time unit 

Possible routes:

1. Berlin → Bonn → Munich (7 hours)
2. Berlin → Magdeburg → Chemnitz → Munich (6 hours)

We are gonna choose route 2 as that takes shortest time to reach to our destination

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%203.png)

## Formulas

Weights = Edges that are numbered on the graph

Weighted graph = A graph that has weights on it’s edges

A weighted graph is similar to an adjacency matrix but instead of true, false or 1,0 which acknowledges there exists a path between two particular nodes, we have the weight of that edge stored at the particular point in the weighted graph. Case where no path exists, we put $\infty$ on it

- Graph as ******weight****** matrix $w$
    - $E = \{(u,v) \ | \ w(u,v) \ne \infty \}$
    - $w(u,v) = d$ — Edge between $u$ and $v$ has weight $d$ (time)
    - $w(u,v) = \infty$ — No edge between $u$ and $v$
- Path $p$ is list of nodes
    - Path between $u$ and $v$: $upv$, where $p$ is the **list** of nodes it has taken you to traverse to get from $u$ to $v$
    - Weight of path: $|u_1, ..., u_n|:= \sum_{i=1}^n w(u_i, u_{i+1})$
    - $|p| = \infty$ means path $p$ is not feasible
- $\delta(u,v)$ — distance between $u$ and $v$ (distance between two nodes that has the minimum weight)
    - $\delta(u,v):= min|upv|$ for all paths $p$
    - $\delta(u,v) = \infty$ — No path from $u$ to $v$

## Weight Matrix

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%204.png)

We can see from the weight matrix graph as if we are going from a → b, we have a value of 3 on the graph, which is equivalent to the time it takes to get to b from a on the graph. Hence a weight matrix represents possible paths with weights instead of true and false.

For nodes that do not have a path to, those are represented by $\infty$

## Shortest Path

The shortest path, is the one with the lowest weight as a summation

- Shortest path: $abdh$ (all nodes from a-h)
- Weight: $|adbh| = 11$
- Distance: $\delta(a,h)=11$ (weight of the shortest path)

<aside>
🙀 Is the weight always equal to the distance in case of a shortest path?

</aside>

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%205.png)

## Multiple Shortest Paths

- Shortest paths are not always unique
- Shortest paths: $acfe, abe$
- However, the distance will always be unique. If you have multiple shortest paths, they will all have exactly the same distance
- Weight: $|acfe| = |abe| = 9$
- Distance: $\delta (a,e) = 9$ (weight of the shortest path)

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%206.png)

## Bellman Ford Algorithm

- Compute shortest paths from $start \ node \ s$ to any node
    - Notation: $\delta(u):= \delta(s,u)$ (We know we are always going to start from $s$ so we can simply represent it by ignoring $s$ and just writing $\delta(u)$
- Start with over-estimate $D$. $D(u) \gt= \delta(u)$
    - $D(s)=0, D(u) = \inf, u \ne s$
    - What we are doing is setting the distance to $\infty$ to over-estimate all distances, apart from the starting node. Start node is 0 units away from the start node because we start at the start node 🤦‍♂️
- Improve estimate until precise
    - For edge $(u,v) (relax\ edge)$
        
        ![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%207.png)
        
    - If path over $(u,v)$ better than current $D(v):$ adjust $D(v)$
    - Basically, can we get a better estimate than what we have previously (remember we start with $\infty$

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%208.png)

- Over estimate shortest path to all vertices
- Iterate over all edges
    - For an edge a → b
        - If we can get to b quicker than we could previously, then update our estimation on b
- Repeat for upto $|V|-1$ rounds

### Example

**Round 1**

![Note that ‘-’ indicate infinity here (for convenience)](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/1.gif)

Note that ‘-’ indicate infinity here (for convenience)

We traverse through all nodes and look at the weights, update their values and store them in a table.

As we had changes in the previous (first) round, we are supposed to have a round 2 of the Bellman Ford Algorithm

**Round 2**

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%209.png)

Since there is nothing to change or optimise, we basically terminate the algorithm with no change having to be made

# Correctness and Complexity of the Bellman Ford Algorithm

## Loop Invariant

The loop invariant is a justification technique to prove a claim about a loop that:

1. holds initially, 
2. is reserved by loop iteration, and
3. implies correctness when loop terminates

<aside>
🙀 What’s correctness?

</aside>

Tom’s steps (?):

1. You make a claim that holds true at start of the loop.
2. We make some assumptions about some intermediate iteration such as iteration `i`
3. We then show that our claim is still holding `i + 1` 
4. To see if execution of a subsequent iteration doesn’t invalidate our claim

Similar to proof by induction ?

![Didn’t really understand, watch the video on this part, again! For the prefix part, if we have a shortedt path that is a→b→c→d (which is a shortest path from a to d, a prefix of that path which is a→b→c is also a shortest path (which is from a→c only)](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%2010.png)

Didn’t really understand, watch the video on this part, again! For the prefix part, if we have a shortest path that is a→b→c→d (which is a shortest path from a to d, a prefix of that path which is a→b→c is also a shortest path (which is from a→c only)

## Negative Weight Cycles

We do not allow negative weight cycles in these paths, and that is a path where the weight of all the edges across the cycle sum up to a negative value and we note that any shortest path is cycle free

- What if negative weight cycle exists?
    - No shortest paths to nodes reachable from cycle
- Bellman-Ford can detect this:
    - Iterate until $D$  does not change
    - If $D$  still changed in $|V|th$ round: **report negative cycle**

## Complexity

- In worst-case, we do $|V|$ rounds because maybe there is a negative weight cycle
- Each round we inspect $|E| \ edges$ and time for relaxing edge: $O(1)$
    
    <aside>
    🙀 Relaxing is when you set a new (improved) value to a node that had a worse (poor) value before (or in an earlier round)
    
    </aside>
    
- Complexity is $O(|V||E|)$
    
    Where $V$ is vertices and $E$ is the number of edges
    

# Negative Weight Cycle with Bellman Ford Example

A negative weight cycle is one where the summation of weights across our cycle is negative, so -7 + 2 + 3  (c,b,d,c) that gives -2, which is negative giving a negative weight cycle

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%2011.png)

![GIF](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/2.gif)
GIF

As from the final product we see a negative value for all vertices, we need to stop as we cannot go on further as we have a negative weight cycle which would go on until infinity (i think?)

And hence we terminate the code

![Untitled](Shortest%20Paths%20b7f11b102a344bfd91a1810002067249/Untitled%2012.png)