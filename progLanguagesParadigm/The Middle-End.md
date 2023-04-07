From the front-end to Intermediate Representations (IR)
Last part of the front-end where we convert the input to an intermediate representation
### Introduction
![[Pasted image 20230406235350.png]]
Syntax analysis has determined that the input program is a valid sentence in the source language.
The outcome of syntax analysis is a parse tree: a graphical representation of how the start symbol of a grammar derives a string in the language
The parse tree can form the basis of the compiler's intermediate representation (IR)
>[!Recall]
>Middle-end may transform the code represented by the IR into equivalent but more efficient code.

However, how can we be sure that we have fixed with the analysis and detection of all possible errors?
#### What is wrong with the following piece of code?
![[Pasted image 20230406235731.png]]
So code that is beyond the scope of the syntax analysis to figure out, which means that the syntax is correct but there is still errors within the written code

#### Beyond Syntax: Context sensitive questions
To generate a code, there might be some questions the compiler needs to answer, such as:
* Is the `ID` $x$ a scalar, an array of a function? Is $x$ even declared
* Are there names that are not declared? If declared are they used?
* Which declaration of `x` does each use reference?
* Is the expression `x-2*y` type-consistent?
>[!Definition]
>Type-consistent: Values in an expression to be of the same type
>Type-checking: The compiler needs to assign a type to each expression it calculates
* In `a[i][j][k]`, is `a` declared to have 3 dimensions?
* Who is responsible to allocate space for `z`? For how long it's value needs to be preserved?
* How do we represent `15` in `f=15` ? Is `f` an integer or real number?
* How many arguments does `foo()` take?
> All of these questions are beyond a context-free grammar, which means context-free grammar cannot give answers to these questions

#### Type Systems
* A value's type is a set of properties associated with it
* The set of types in a programming language is called a **type system**
* **Type checking** refers to assigning/inferring types for expressions and checking that they are used in contexts that are legal
* Components of a type system:
	* Base or Built-in types (e.g. numbers, characters or booleans)
	* Rules to: Construct new types; determine if two types are equivalent; infer the type of source language expressions
* Type checking: (Depending on the design of the language and the implementation)
	* At compile-time: Statically checked
	* At run-time: Dynamically checked
	![[Pasted image 20230407012150.png]]
If the language has a 'declare before use' policy, then inference is not difficult. The real challenge is when no declarations are needed.

>[!Static vs Dynamic Checking]
>Statically checked type systems are those that check the type safety of a program at compile-time, before it is run. This means that the type of each variable and expression is known and verified by the compiler before the program is executed. If a type error is found, the compiler will not generate executable code and will report an error message to the programmer. This approach can lead to faster and more efficient code execution because the type checking is done ahead of time and does not need to be performed at runtime.
>
>On the other hand, dynamically checked type systems check the type safety of a program at runtime, as it is executed. This means that the type of each variable and expression is checked at runtime, and if a type error is found, an exception or error is raised, halting the program execution. This approach can provide greater flexibility and ease of use, but can also result in slower program execution because the type checking is performed during runtime.

#### Dealing with context-sensitive questions
Questions in [[#Beyond Syntax: Context sensitive questions]] are part of context-sensitive analysis (which is also known as semantic analysis)
* The answers depend on values, not syntax
* Questions and answers depend involve local non-information
* Answers may also involve computation
How can we then answer these questions:
* Use formal methods:
	* Use context-sensitive grammars: Many important questions would be difficult to encode in a context-sensitive grammar and the set of rules would be too large to be manageable (e.g. the issue of declaration before use)
	* Attribute grammars: People try to use a context-free grammar by adding something extra that would allow some answers to context sensitive questions
* Use ==ad hoc== techniques: 
	* **Symbol tables** and ad hoc, syntax-directed translation

>[!Definition]
>Ad hoc: Done for a specific purpose

#### Symbol Tables
A data structure which acts as a central repository of facts
* Could be one symbol table or a set of symbol tables.
It is first created during [[Lexical Analysis]]
It is retained during compilation (useful for debugging too)
Using a symbol table one can check basic semantic questions by associated a snippet of code with each production rule that would execute each time the parser reduces that prodcution. Examples:
* Code that checks if a variable is declared prior to use 
* Code that checks that each operator and it's operands are type-compatible
Allowing arbitrary code in such code snippets provides flexibility
This type of evaluation fits nicely with LR(1) parsing
How is the symbol table populated?

##### What information is stored in the symbol table?
What items to enter in the symbol table?
* Variable names
* Defined constants 
* Procedure and function names
* Literal constants and strings
* Source text labels
* Compiler generated libraries
What kind of information might the compiler need about each item?
* Textual name
* Data type
* Declaring procedure
* Storage information
* Depending on the type of the object, the compiler may want to know list of fields (for structures), number of parameters and types (for functions, e.t.c)
In practice, many different tables exist

>[!Note]
>Symbol table information is accessed frequently: hence, efficiency of access is critical

In lexical analysis, we can detect symbols and variables, and add them directly to the symbol table. Along with that, we can also add all above mentioned items directly to the symbol table.

#### Organising the symbol table
1. Linear list: Simple approach, has no fixed size; but inefficient. A lookup may need to traverse through the entire list, this takes $O(n)$
2. Binary tree: 
	* An unbalanced tree would have similar behaviour as a linear list (this could arise if symbols are entered in a sorted order)
	* A balanced tree (path length is roughly equal to all it's leaves) would take ($Olog_2n$) probes per lookup (worst-case). Techniques exist for dynamically rebalancing trees
3. Hash tables: 
	* Uses a hash function to map names into integers; this generates a table index to store information. Potentially $O(1)$, but needs:
		* Inexpensive hash function, with good mapping properties and
		* a policy to handle cases when several names map to the same single index

#### Example of a Hash Table
![[Pasted image 20230407022804.png]]
#### Other Issues
* Choosing a good hash function is of paramount importance:
	* Split names into four-byte chunks, XOR the chunks together and take this number and modulo 2048 (this is the symbol table size)
* Lexical Scoping:
	* Many languages introduce independent name scopes:
		* C, for instance, may have global, local, static(file) and block scopes
		* Pascal: nested procedure and definitions
		* C++, Java: Class inheritance, nested classes, packages, namespaces, etc.
		* Namespaces allow two different entities to have the same name within the same scope: E.g. in Java, a class and a method can have the same name
	* The problem:
		* At point x, which declaration of variable y is current?
		* Allocate and initialise a symbol table for each scope
	* By far, easiest to have a single global namescope (but not most efficient), yet another trade off.

### The Intermediate Representation
Once the source code has been checked for errors, the compiler needs to convert it into a convenient intermediate representation. This representation can also be used to:
- Facilitate retargeting
- Enable machine-independent code optimisations and/or aggressive code generation strategies.
Design issues:
* Ease of generation, ease of manipulation, cost of manipulation, level of abstraction, size of typical procedure
* Decisions with the IR design have major effects on the speed and effectiveness of compiler
A useful distinction (both types co-exist in practice):
* An internal representation to represent code
* An internal representation to capture properties of the code that may be useful for analysis
#### Taxonomy of IR
Categories of IRs to represent code by structure:
* Graphical (structural): such as trees
	* Close to source code
	* Node and edge structures tend to be large
* Linear: Pseudocode for some abstract machine:
	* Large variation
	* Close to target code
* Hybrid: Combination of both
Auxiliary (mostly graphical) representations may be used to capture properties of the code (but not the whole code)

>[!Note]
>There are no universally good choices for IR. The right choices depend on the goals of the compiler

#### From Parse Trees to Abstract Syntax Trees
Why do we not want to use parse trees?
	Has a lot of unnecessary information
How to convert that into an abstract syntax tree?
* Traverse the tree in post order (first traverse node x's subtrees in left-to-right order and then node x)
* Remove non-terminal symbols
![[Pasted image 20230407034202.png]]

### Abstract Syntax Trees
An abstract syntax tree (AST) is the procedure's parse tree with the non-terminal symbols removed
##### Example: `x-2*y`
![[Pasted image 20230407034331.png]]

The AST is a near source-level representation
Source code can be easily generated: perform an inorder treewalk - first the left subtree, then the root, then the right subtree - printing each node as visited
Issues: Traversals and transformations are pointer-intensive; generally memory intensive
Example: `Stmt_sequence -> stmt; Stmt_sequence | stmt`
* Atleast three AST versions for stmt;stmt;stmt;
	![[Pasted image 20230407034718.png]]

#### Three Address Code
A term used to describe many different representations: each statement is a single operator and at most three operands
Example: $if (x>y) \ then \ z=x-2*y$ becomes:
```assembly
t1 = load x
t2 = load y
t3 = t1>t2
if not (t3) goto L
t4 = 2 * t2
t5 = t1 - t4
z = store t5
L = ...
```
>[!Note]
>Array addresses have to be converting into a single memory access. Example, `a[i]` will always require a `load i` and then something like: `t1 = i*sizeOf(i);` `load base_address(a)+t1`
 
Advantages: Compact form, makes intermediate values explicit, resembles many machines
Storage considerations (Until recently compile-time was an issue)
![[Pasted image 20230407035630.png]]
#### Other Linear Considerations
* Two address code: This is more compact, in general it allows statements of the form `x = x<op>y` (single operator and at most two operands)
```assembly
load r1,y
loadi r2,2
multiply r2,r2, r1
load r3, x
sub r3,r3,2
```
* One address code: (Aka stack machine code) is more compact (more than two address?). Would be useful in environments where space is at a premium (has been used to construct byte interpreters for Java):
```assembly
push 2
push y
multiply
push x
subtract

// note that when multiply, it multiplies the first 2 elements on top of stack, should be similar to every other operator too
```

#### Auxiliary Graph Representation
1. Control-flow Graph (CFG): Models the way that the code trasnfers control as a result of conditional or loop statements.
	* Node: A single basic block (= a maximal straight line of code)
	* Edge: Transfer of control between basic blocks
	* (Captures loops, if statements, case, goto)
2. Data Dependence Graph: Encodes the flow of data
	* Node: Program statement
	* Edge: Connects two nodes if one uses the result of the other
	* Useful in examining the legality of program transformations
3. Call Graph: Shows dependences between procedures
	* Useful for interprocedural analysis

#### High-level Code Optimisations
Once the intermediate representation is in place, the compiler may try to apply a series of optimising transformations
Goal: Improve program performance:
* This may also reduce size of code, power consumption, e.t.c.
Issues:
* Legality: Must preserve the meaning of program
	* Externally observe meaning may be sufficient / may need flexibility
* Benefit: Must improve performance on average or common cases
	* Predicting program performance is often non-trivial
* Compile-time cost justified: List of possible optimisations is huge
	* Interprocedural optimisations (O4 (?))

##### Optimising Transformations
Finding an appropriate sequence of transformations is a major challenge: modern optimsers are structured as a series of passes:
* Optimisation 1 is followed by optimisation 2, optimisation 2 is followed by optimisation 3, and so on...
Transformations may improve program at:
* Source level (algorithm specific)
* IR Level (machine-independent transformations)
* Target code (machine-dependent transformations)
Some typical transformations:
* Discover and propagate some constant value 
* Remove unreachable/redundant computations
* Encode a computation in some particularly efficient form

#### Classification
By scope:
* Local or within a window of instructions(peephole (wtf))
* Loop-level: On one or more loops or loop sets
* Global: For an entire procedure (e.g. function, method, etc)
* Interprocedural: Across multiple procedures or whole program 
By machine information used:
* Machine-independent (high-level)
* Machine-dependent
By effect on program structure:
* Algebraic transformations(e.g. `x+0, x*1, 3*z*4,...`)
* Reordering transformations (change the order of 2 computations)
	* Loop transformations: loop-level reordering the transformations
##### Some transformations:
* Common subexpression elimination:
	* Say for instance, `x+y` is redundant iff along every path from the procedure's entry it has been evaluated and it's constituent subexpressions (`x,y`) have not been redefined. In simpler words, assign the value of `x+y` to a variable and just use that variable again instead of calculating the sum again and again.
* Copy propagation:
	* After a 'copy' statement, `x=y`, trying to use `y` as much as possible (where instead could have used `x`, is this what is meant by here?)
* Constant propagation: 
	* Replace variables that have constant values with these values
* Constant folding:
	* Deduce a value that is a constant, and use the constant instead
* Dead-code elimination:
	* A value is computed but never used, or there is code in a branch never taken (may result after constant folding). Example unused/unaccessed variables (maybe?)
* Reduction in strength:
	* Replace an expensive operation with a cheaper operation
* Loop-invariant code-motion:
	* Detect statements inside a loop whose operands are constant or have all their definitions outside the loop - move out of the loop
##### Examples
![[Pasted image 20230407054300.png]]
> [!info]
> I think these are great examples

#### Loop Transformations - Loop Unrolling
* Change: `for(i=0;i<n;i++)` to `for(i=0;i<n-s+1;i+=s)` and replicate the loop body `s` times (changing also `i` as needed to `i+1, i+2, ...`). Will need an 'epilogue' if `s` does not divide `n`
* Helps with low-level instruction scheduling, minimises loop overhead
![[Pasted image 20230407054852.png]]

### Summary
![[Pasted image 20230407054938.png]]

