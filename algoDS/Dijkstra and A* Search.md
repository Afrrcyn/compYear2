# Dijkstra and A* Search

# Dijkstra’s Algorithm

Getting to a path as fast as possible

## Pseudocode

![Untitled](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/Untitled.png)

In Dijkstra’s algorithm, we explore the node, with the minimum distance estimation.

<aside>
💡 Prefix of a shortest path is itself a shortest path

</aside>

In the pseudocode above, $D$  is held as a priority queue, where it is ranked according to the distance estimation on the node. To get to the next node to explore, we pop from the top of the priority queue, and we only add to the priority queue when we discover a node, as hence only reachable nodes are actually explored.

The $relax(u)$ function from what I think does is relaxes all edges and each time updates the estimate if there is a shorter path found. 

![ezgif-2-91715b9505.gif](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/ezgif-2-91715b9505.gif)

The above gif shows how we could get to Munich in the shortest time. Nodes are discovered in D, and once discovered are then sent to F (Finished exploring).

Also we can see an update in for Frankfurt, before it had a path from Berlin to Frankfurt via Bonn which was 2 + 3 ( = 5), now as we have explored Hamburg, we can see the path as from Berlin to Frankfurt through Hamburg, which is 2 + 1 (=3) which is faster, hence we update it’s value

## Implementation

Using a priority queue:

- Relaxation means a node’s value may decrease
- We need a decrease key operation, how? We can use a MinHeap
- Use a MinHeap and restore heap-property after pop, insertion, relaxation operations. We can then restore the heap property using a sift up operation (what is a sift up operation?)

By adding nodes as they are discovered:

- We won’t explore any unreachable nodes
- No need for infinity in the priority queue

Use predecessors to backtrack and find shortest path

### MinHeap

A MinHeap is a type of binary heap where the parent node is always smaller than or equal to its child nodes. This allows for the root node to always be the minimum value in the heap. In an implementation, the MinHeap would typically have methods for inserting a new value, removing the minimum value, and restoring the heap property after an operation.

## Complexity

1. Operations:
- Every edge is relaxed ************************at most once************************, because we visit each node once
- Every node is added and removed from the priority queue ****************at most once****************
1. Cost of relaxing, addition, extraction within the min heap priority queue: $O(log|V|)$
2. Worst-case complexity: $O((|E|+|V|)\ log(|V|)$, which is better than the Bellman-Ford Algorithm

# A* Algorithm

## Pseudocode

![Untitled](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/Untitled%201.png)

This means for example in the Germany train example, we would be ranking higher the stations that are closer to Munich than the ones that are further away (efficient)

Secondly, we terminate the program once we reach the goal destination, which helps save a lot of search space

![GIF](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/ezgif-5-ead36d1882.gif)

GIF

See that the it stops once Munich has been discovered

We can see that the A* algorithm completely avoided Hamburg, Bond and other cities as those paths were not the shortest paths to Munich

## Heuristic Admissibility and Monotonicity

### What is meant by Heuristic Admissibility and Monotonicity?

<aside>
💡 **Heuristic:** A heuristic is a problem-solving strategy or technique that uses a practical approach to finding a solution instead of using an algorithmic or formal method. In the context of search algorithms, a heuristic function is used to estimate the distance or cost to the goal node from a given node in the search space. This function is used to guide the search towards the goal in a more efficient way.

</aside>

> Heuristic admissibility is a property of an algorithm in which the heuristic function never overestimates the cost of reaching the goal node from any given node. In other words, the heuristic function always provides a lower bound on the actual cost of reaching the goal.
> 

> Monotonicity is a property of an algorithm in which the sum of the heuristic function and the actual cost of reaching the goal from any given node is always less than or equal to the sum of the heuristic function and the actual cost of reaching any successor of that node plus the cost of moving from the current node to its successor. In other words, the heuristic function is consistent and never increases as we move along the path from the current node to the goal node.
> 

To use an heuristic, it must be:

- Admissible: It must not overestimate the true cost of reaching the destination $h(u) \le \delta(u,t)$
- Monotone: i.e. Fits the triangle inequality

$h(a) \le w(a,b) + h(b)$

![Untitled](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/Untitled%202.png)

## Complexity

Exactly the same as Dijkstra, gonna copy paste it 

Worst-case complexity: $O((|E|+|V|)\ log(|V|)$, which is better than the Bellman-Ford Algorithm

We save time and memory as we save time getting to our destination quicker and space as we are not storing other nodes hence saving space

# Correctness

![Untitled](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/Untitled%203.png)

I1 - If a node $u$ is finished, then it’s estimate is precise. That also means that the estimate of the shortest path weight from the source to $u$ is precisely the correct value

I2 - For a node $v$ in the graph which follow $u$ the estimate that it is assigned will be no greater than the precise path weights up to $u$ plus the weight of the edge that connects $u$ and $v$

I3 - For node $u$ in graph, the estimate given to it will be no less than the precise shortest path value, i.e. there will always be an overestimation

![Untitled](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/Untitled%204.png)

### Correctness

![Untitled](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/Untitled%205.png)

Watch the video again 😢

### Correctness of A*

A* is an extension of Dijkstra 

![Untitled](Dijkstra%20and%20A%20Search%202c963ea9a9d8492285c8d49a41254456/Untitled%206.png)