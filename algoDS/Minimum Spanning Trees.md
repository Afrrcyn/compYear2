# Minimum Spanning Trees

# Introduction to MST

A minimum spanning tree will ensure that all nodes of the graph are connected, but the sum of the weight of the edges that are used are actually minimised

An MST will connect all nodes with the sum weight of the edges minimised. We can see that all cities are connected (green line)

Here, MST has weight of 13

So, every city can be connected by a railway line at the minimum cost of 13

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled.png)

### A bit more formally

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%201.png)

## Generic Greedy Algorithm

**Greedy Strategy:**

Growing the minimum spanning tree one edge at a time, which is not always globally optimal. 

![Greedy algorithm on the left](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%202.png)

Greedy algorithm on the left

How the algorithm works:

We are going to assume that $A$ is a subset of a minimum spanning tree, that exists but we may not have found it yet. While $A$ does not form a spanning tree, we are going to find an edge that is safe and then we are going to add it to our set and we are going to then go ahead and iterate the loop. Does it form a spanning tree? Yes? Terminate. No? Pick another edge that is safe

How do we find out what is a safe edge?

## Recognising Safe Edges

Undirected graph $G = (V,E)$

- $E = \{(u,v)\ | \ w(u,v) \ne \infin  \}$

$A \subseteq E$  included in some MST for $G$

$(S, V-S)$ any cut of $G$ that respects $A$

$(u,v)$  a light edge crossing $(S, V-S)$

$(u,v)$  is safe for $A$ 

Explaining the above algorithm:
We have an undirected graph $G$, and from that graph we have selected a subset of edges, $A$.

We are going to say that the subset of the edges are all included in the minimum spanning tree, but we do not know if it’s the complete minimum spanning tree yet or not. Hence, we are going to make a **cut** in the graph that respects $A$, that is to say that the cut removes this minimum spanning tree in progress, and does not chop it up at all. Now the light edge (the part that we have cut?) which is the edge that connects our existing minimum spanning tree in progress to some other node, that is not within this minimum spanning tree in progress. And that edge is one that has the minimum weight that crosses the cut. So this **light edge** is actually the safe edge and that is because it is connecting our minimum spanning tree to something else with the minimum weight possible, so it is not introducing any extra weight there and it’s also not introducing any cycles. So by that, because it’s already connecting something that’s inside, the minimum spanning tree, with something that is definitely outside of the minimum spanning tree.

## Correctness (Loop Invariant)

- Prior to each iteration: $A \subseteq$ some MST
- Each step: determine $(u,v) \notin A$ to add to $A$ without violating this invariant:
    
    What we mean by this is that when we union the two sets $A$  and the set containing our edge which is also a subset of $T$, which is our minimum spanning tree. And because we know that a union with this new edge is a subset of a spanning tree, therefore it is a safe edge initialisation
    
    - $A\ \cup \ \{(u,w) \} \subseteq T$
    - $(u,v)$ then a *safe* edge
- Use the loop invariant as follows:
    1. Initialisation
        - A trivially satisfies the loop invariant
    2. Maintenance 
        - Add only safe edges
    3. Termination
        - All edges added to $A$ are in a MST, so $A$ returned must be a MST
        - It terminates when we know that it forms a spanning tree, so therefore a set of edges of spanning tree that it is returned, must be a minimum spanning tree

## Cuts and Light Edges

A cut $(S, V-S)$ of an undirected graph $G = (V,E)$ is a partition of $V$

A cut $respects$ a set $A$ of edges if no edge in $A$ crosses the cut

An edge crosses a cut if one of it’s endpoints is in $S$ and the other in $V - S$

An edge is a *light* edge crossing a cut if it’s weight is the minimum of any edge crossing the cut

- More than one light edges can exist

What happens is, we split the graph into two parts, the first part is the bit that’s cutaway, the second part is the remainder of the nodes of the graph $V - S$. Like a scissor, you cut a graph, one part is $S$, another is $V -S$

If any of those edges, do not cross the cut, if that is the case then the cut respects that set of edges

> When we say that a cut respects a set of edges, we mean that none of the edges in the set cross the cut. In other words, the cut separates the graph into two parts, one containing the nodes in $S$ and the other containing the nodes in $V-S$, and there are no edges in the set that cross the cut between these two parts. (Notion AI)
> 

So an edge crossing over the cut is, where one of the endpoints is in $S$  and one of the endpoints is not is $S$

A **light edge** is an edge that is crossing the cut where the weight is at a minimum of all the edges crossing the cut. We can have more than one light node edge existing, but we can also have many edges crossing. But the **light edge is the one that has the minimum weight**.

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%203.png)

# Kruskal’s Algorithm

## Pseudocode

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%204.png)

What difference we can see is that in here, we order edges by weights. So the first edge we are going to get is going to have the lowest weight

The idea of Kruskal’s algorithm is that we order the edges, by their weight. Pick the smallest one and it it’s connecting two new edges together that have not already been connected to the minimum spanning tree, then we will have that edge (?)

At the start of the algorithm, $A$  is an empty set in our spanning tree. For each node, we make it into a singleton disjoint set. And we pick each edge off as per it’s ordered weight ranking. So if $u$ and $v$  are in different sets, either means that one of them is by themselves, or that they are just not connected. If they are not connected, then we will take that edge, have it in our minimum spanning tree, and will union the two sets together, so we could keep a record of them being connected.

<aside>
💡 Singleton refers to a set that contains only one element. In computer programming, it is often used to represent a unique object or instance of a class.

</aside>

### Disjoint Sets

1. $Make-Set(x)$ — create a new set whose only member (and thus representative) is x
- Require that x is not already in some other set

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%205.png)

$x.rank$ — an upper bound on the number of edges in the longest simple path from a descendant leaf to $x$

1. $Find - Set (x)$ — returns a pointer to the representative of the unique set containing $x$
- Path compression: each node on the find path points directly to the root

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%206.png)

1. $Union(x,y)$ — unites the sets that contain $x$  and $y$ into a new set that is the union of these two sets
    
    ![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%207.png)
    
    - Implementation using the union-by-rank heuristic

### Union by Rank Example

![What would happen if we apply union on these two disjoint sets?](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%208.png)

What would happen if we apply union on these two disjoint sets?

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%209.png)

Note that root with smaller rank points to root with larger rank

We can see the result of union on the two sets. 

$c$ has a rank of 1 whereas $f$ has a rank of 2 (look at the number of children?). Which means $c$  is going to point to $f$ when we union them as $f$  would be ranked higher

## Kruskals Algorithm

Note: We also want to prevent from having a cycle present

![ezgif-4-54c7288445.gif](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/ezgif-4-54c7288445.gif)

We were getting a cycle at from MD to F as well as MD and F were already a part of that set and not disjoint sets so no need to put them together

## Complexity

Depends on the implementation of the disjoin-set data structure

$O(|E|  \ log|V|)$ — using the path-compression and union-by-rank heuristics

# Prims Algorithm

## Pseudocode

Intuition:

- Start from an arbitrary node $r$
- In each step, add to $A$ a light edge that connects $A$ to an isolated vertex
- Similar to Dijkstra

Execution:

- $v.key$ is the minimum weight of any edge connecting $v$ to a node in the tree
- $Q$ is a min-priority queue based on $key$
- $u.\pi$ is the parent of $u$ in the tree
- $A = \{ (v,v.\pi):v \in V - \{ r\} - Q \}$
- while loop executes $|V|$  times

## Algorithm

![Untitled](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/Untitled%2010.png)

![Note that we could have taken either 3 at our 4th iteration](Minimum%20Spanning%20Trees%20f26abdd12a1149bda0ba697238d791ec/JnJ5Nqu.gif)

Note that we could have taken either 3 at our 4th iteration

## Complexity

Depends on the implementation of the min-priority queue $Q$

$O(|E| + |V| \ log|V|)$ using a Fibonacci heap to implement $Q$