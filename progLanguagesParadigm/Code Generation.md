### Introduction
The back-end
Goal: Produce target code for a given platform and optimise it
![[Pasted image 20230408094156.png]]
The task of the back-end is to take the partly optimised code intermediate representation and generate target code. We face two issues here:
1. Select target language instrcutions - a pattern matching problem
2. Optimise in line with what the target platform offers
We will focus on:
* Instruction selection: map intermediate representation into target code
* Register allocation: decide which values will reside in registers
* Instruction scheduling: Reorder operations to hide latencies (tl dr; what reordering of instructions would produce the fastest code)
#### Instruction Selection
Characterstics:
* Essentially pattern matching: match the intermediate representation to target code
	* For example, if mapping `if` statement, that may interact with something the code interacts with later within the code
* Can make locally optimal choices; global optimality is NP-complete
* Assume there are enough registers
![[Pasted image 20230408095234.png]]
##### Code Generation for Arithmetic Expressions
![[Pasted image 20230408095441.png]]
>[!Post-order Traversal]
>First scan the left sub-tree, then the right, then the root node. After all that generate code

##### Issues with Arithmetic Expressions
What about values already in registers?
* We do not have to load them again but only modify the identifier case (? what does this mean)
Why the left sub-tree and not the right?
* (`2*y-x`, `x-2*y`, `x+(5+y)*7`): Evaluate the most demanding (in registers) subtree first (Sethi-Ullman labelling schemes)
>[!Why left and not right?]
>The reason for this is that in many cases, the value of an arithmetic expression depends on the values of its sub-expressions. Therefore, we need to generate code for the sub-expressions first and then combine their results to produce the final result.
>The Sethi-Ullman labeling scheme is a technique for determining the most efficient way to evaluate an expression in terms of the number of registers needed. The basic idea is to assign labels to each node in the expression tree that indicate the number of registers needed to evaluate that node's subtree.

> Kind of makes sense

Could be a second *path* to minimise register usage/improve performance, for example two computations instead of just assigning the computed value to a variable (i think)

The compiler can take advantage of commutativity and associativity to improve code for integers

Problem: Generate code that multiplies an integer with an unknown using only shifts and adds. Example:
```
325*x = 256*x+64*x+4*x+x or (4*x+x)*(64+1)
-- When is it better not to use integer multiplication
-- notice left expression has three multiplications whereas the right has just two
```

#### Array References

>[!Note]
> There is only one dimension of memory space
> When generating code, everything will be converted into blocks in memory, two-dimensional arrays will be converted into one-dimension array

Agree on a (convention of) memory storage scheme for array elements:
* Row-major order: Layout as a sequence of consecutive rows
	`A[1,1], A[1,2], A[1,3], A[2,1], A[2,2], A[2,3]` 
* Column-major order: Layout as a sequence of consecutive columns
	`A[1,1], A[2,1], A[1,2], A[2,2], ...` (Fortran's approach)

We can easily derive formulaes to compute an array element starting from the first element of the array(base). Example for an array defined as size `[n][m]`, element `[i][j]` is given by:
* Row-major: `base + (i*(m+1)+j)*w`
* Column-major: `base + (j*(n+1)+i)*w`
	Where `w = sizeOf(single_array_element)`

#### Control Flow Statements
* If `Expr` then `stmt1` else `stmt2`:
	1. Evaluate the `Expr` to true or false
	2. If true, go to then; branch **around** else
	3. If false, branch to else, fall through to next statement
		(if not `Expr` then `br L1; stmt1; br L2; L1: stmt2; L2;... )` (wtf is this 😭)
* `While` loop; `for` loop; or `do` loop
	1. Evaluate `Expr`
	2. If false, branch beyond end of loop; if true fall through 
	3. At end, re-evaluate `Expr`
	4. If true, branch to top of loop body, if `Expr` then `br L1;L2:...`
* Complex boolean expressions can also be evaluated using treewalk but not only, for example:
	* Short-circuit evaluation: The C expression `x!=0 && y/x>1` relies on short-circuit evaluation for safety

#### Procedure / Functions / Methods
* Procedures are key to building complex software but they are a high-level abstraction not supported by low-level hardware.
* The compiler needs to generate code that emulates the behaviour of procedures. For every call:
	* Pre-call will add instructions to allocate memory for procedure specifics, which is de-allocated in post-return. Prologue will initialise variables and so on...
* This is costly: In java, an optimisation called **method inlining** is trying to embed the code for methods in the code where they are called from
![[Pasted image 20230408102754.png]]
The compiler needs to add code that will make sure that the variables defined in procedure Q will occupy space in memory only for as long as Q is executed and then they could be removed from memory.

#### Register Allocation
The initial code makes use of an unbound (infinite) number of registers (virtual registers) but the machine has only a limited number of registers (physical registers), say $k$
The task:
* Produce correct $k$ register code
* Minimise the number of loads and stores (**spill code**) and their space
* The allocator must be efficient (e.g. no backtracking)
Assume a RISC-like (three-address) type of code
##### Background (Definitions)
* Basic block: A maximal length segment of straight-line (i.e. branch-free code). (Importance: strongest facts are provable for branch-free code; problems are simpler; strongest techniques)
* Local register allocation: Within a single basic block (BB)
* Global register allocation: Across an entire procedure (many BBs)
* Allocation: Choose what to keep in registers
* Assignment: Choose specific registers for values
* Modern processors may have multiple register classes:
	* General-purpose, floating-point, branch target...
	* Problem: interaction between classes - assume separate allocation for each class
* Complexity: Only simplified of local allocation and assignment can be solved in linear time. All the rest (including global allocation - even for one register - and most sub-problems) are NP-complete

#### Liveness and Live Ranges
Problem: What is the number of registers needed in a basic block?
* Naïve: all occurrences of a variable to the same register
* Realistic: Compute a set of live ranges and use their name space
A value of a variable is **live** between it's definition and it's uses:
* Find definitions `x <- ...` and uses `... <- x ...`
* From definition to last use is the 'live range'
* Can represent live range as an interval `[i,j]` in basic block
Overall instructions in the basic block, let:
* MAXLIVE be the maximum number of values live at an instruction
* k, the number of physical registers available
	* If MAXLIVE $\le$ k, allocation is trivial 
	* If MAXLIVE $\ge$ k, some values must be saved to memory ('spilling')
![[Pasted image 20230408112430.png]]

#### Basic (Local) Register Allocation
**Top-down register Allocation**
- Reserve registers for the most frequently used values. All other values are loaded from/stored to memory when needed.
- Problem: A value heavily used in one part of the code but rarely used in the other part of the code will still occupy a register.
**Bottom-up register Allocation**
- Keep a pool of registers, assign one register when a value is initialised (start of the live range); return register to the pool at the end of the live range
- If at some point we need to assign a register but there is no register in the pool, select the register that is used farthest in the future, store it's value and return it, (a process known as 'spilling')

#### Graph Colouring
##### Background
**The Problem:**
A graph is said to be $k$-colourable iff the nodes can be labelled with integers $1..k$ so that no edge connects nodes with the same labels
**Example:**
![[Pasted image 20230408144610.png]]
##### Register Allocation via Graph Colouring
**Idea:**
- Live ranges that do not interfere can share the same register 
**Algorithm:**
1. Construct live ranges
2. Build interference graph
3. (try to) construct a k-colouring of the graph:
	1. If unsuccessful (i.e. we need more than k colours), choose values to spill on the basis (on the basis of some cost estimates) and repeat from start (choosing what to spill is a critical issue)
4. Map colours onto physical registers

##### Build the Interference Graph
**Interference:**
- Two values interfere if there exists an operation where both are simultaneously live; if they interfere they cannot occupy the same register
The interference graph:
- Nodes: Represent values (or live ranges)
- Edges: Represent individual interferences
![[Pasted image 20230408144857.png]]

##### Construct a k-colouring graph
1. Top-down Colouring
	- Rank the live ranges (nodes)
		- Possible ways of ranking; number of neighbours, spilling cost, etc
	- Follow the ranking to assign colours:
		- For each node, pick the first colour that is not used by the node's neighbour
	- If the compiled cannot find a k-colouring, it needs to spill some valie (store after definition, load before each use), something that splits the live range
		![[Pasted image 20230408145304.png]]
2. Bottom-up Colouring
	?

#### The Big Picture
![[Pasted image 20230408145426.png]]

### Instruction Scheduling
- Instruction Latency: Length time elapsed from the time the instruction was issued until the time the results can be used. For an this may be 1 cycle, for multiplication, could be more.
- The problem: 
	- Given a code fragment for some target machine and latencies for each individual instruction, reorder operations to minimise execution time.
	- Recall: Modern processors may have multiple functional units
- The task:
	- Produce correct code that minimizes execution time
##### Background
The compiler knows that many operations have delay latencies when executed. The details are processor-related but the result may appear with delay (instruction latency):
- For example, a load is scheduled, it's result appears $x$ cycles later
- However, the execution continues unless the result is referenced.
- Premature reference of the result will cause the hardware to stall (how?)
Modern machines can issue several operations per cycle
Execution time is order-dependent
Overview of a solution:
- Move loads back at least $delay$ slots from where they are needed, but this increases register pressure (i.e. more registers may be needed)
- Ideally, we want to minimise both hardware stalls and added register pressure
![[Pasted image 20230408151005.png]]
What is the principle of the idea to execute things faster?
	- Costly instructions are executed as early as possible, because we want to minimise the impact they have on the code later.
#### Instruction Scheduling for a Basic Block 
1. Build a precedence (data dependence) graph
2. Compute a priority function for the nodes of the graph
3. Use list scheduling to construct a schedule, one cycle at a time:
	1. Use a queue of operations that are ready
	2. At each cycle:
		1. Choose a ready operation and schedule it
		2. Update the ready queue

>[!Note]
>This is a greedy heuristic. 
>Greedy heuristic: An algorithmic technique in which an optimisation problem is solved by finding locally optimal solutions

![[Pasted image 20230408151633.png]]
##### Step 2: Compute a priority function
Assign to each node a weight equal to the longest delay latency (total) to reach a leaf in the graph from this node (include latency of current node)
- $Weight_i = latency_i + max(weight_{all \ successor \ nodes})$  
	![[Pasted image 20230408152009.png]]

##### Step 3: (Local) List Scheduling
![[Pasted image 20230408153059.png]]
Use the following schedule table to understand how precedence is applied in the example above:
![[Pasted image 20230408153525.png]]
> Would also want to see how he explains it: https://video.manchester.ac.uk/embedded/00000000-7c11-941a-0000-017811954ae0

#### More on List Scheduling
Two distinct classes of list scheduling:
- Forward list scheduling: Start with all available operations; work forward in time (the algorithm presented here)
- Backward list scheduling: Start with leaves; work backwards in time 
- Folk wisdom is to try both and keep the best result
Variations on computing prioirty functions:
- Maximum path node containing node (decreases register usage)
- Prioritise critical path
- Number of immediate successors or total number of descendants
- Increment weight if node contains a last use (shortens live ranges)
With a small modification, the algorithm can deal with cases where multiple operations can be scheduled at the same time

#### Further Issues
The back-end is essentially a sequence of optimising transformations that aim to generate optimised target code
Register allocation interferes with instruction scheduling:
- The former before the latter restricts the choices for scheduling
- If the latter before the former and register allocation has to spill registers, the whole (carefully done) schedule changes
- The same can be said about instruction selection too
Most of the active compiler research revolves around areas of optimisation in relation to middle/back-end. Trying to optimise in relation to new target platforms is always a big challenge
