[[COMP26020]]

Syntax analysis, also known as ==parsing==
### Introduction
![[Pasted image 20230406011707.png]]

After lexical analysis, the compiler can now produce tokens, i.e. characters of the input have been grouped into tokens. Syntax analysis will then try to understand if these tokens are syntactically correct. If they are in a correct order and do they form valid sentences of the input language.
Syntax analysis therefore discovers a derivation for some sentence.

Not all languages can be defined by ==regular expressions==
The descriptive power of regular expressions has limits
* Regular expressions (RE) cannot be used to describe balanced or nested constructs: Example set of all strings of balanced parantheses {(), (()), ((())), ...}
* In regular expressions, a non-terminal symbol cannot be used before it has been fully defined.

#### Chomsky's Hierarchy of Grammar:
1. Phrase structured: Natural languages that we use everyday
2. Context sensitive: Number of left hand side symbols are $\le$ number of right hand side symbols
3. Context-free: The left hand side symbol is a non-terminal
4. Regular: Only rules of the form: $A \implies \epsilon, A \implies a, A \implies pB$ are allowed
Regular languages $\subset$ Context-Free languages $\subset$ Cont.Sens.Ls $\subset$ Phr.Str.Ls
Regular languages are the most constrained type of languages.

#### Expressing Syntax
Context-free syntax is specified with a context-free grammar

> [!Recall]
> A grammar, $G$, is a 4-tuple $G = \{S,N,T,P\}$ where:
> $S$: Starting symbol
> $N$: Set of non-terminal symbols
> $T$: Set of terminal symbols
> $P$: Set of production rules

##### Example: The CatNoise Grammar
$CatNoise \implies CatNoise \ miau$ **rule 1**
						| $miau$ **rule 2**
We can use the CatNoise grammar to create sentences, for example: 
![[Pasted image 20230406170221.png]]

Such a sequence of rewrites is called ==derivation==
The process of discovering a derivation for some sentences is called ==parsing== (Syntax analysis)

#### Derivations and Parse Trees
Derivation: A sequence of derivation steps:
* At each step, we choose a non-terminal symbol to replace
* Different choices can lead to different derivations

Syntax analysis is interested in two types of derivations:
1. Leftmost derivation: At each step, replace the leftmost non-terminal
2. Rightmost derivation: At each step, replace the rightmost terminal
(we do not care about randomly-ordered derivations)

A parse tree is a graphical representation for a derivation that filters out the choice regarding the replacement order
##### Example
The CatNoise derivation
![[Pasted image 20230406170756.png]]

#### Derivations and Precedence
For example if we are asked to generate a lefthand and righthand derivation parse trees for the expression: $x-2*y$, the lefthand derivation gives us $x-(2*y)$ whereas the righthand derivation would give us $(x-2)*y$
The two derivations give rise to an issue, there is no notion of precedence or implied order of evaluation, therefore the grammar is ambiguous, which is not a good property for context-free grammar.

#### Ambiguity
1. A grammar that produces more than one parse tree for some sentences is ambiguous
2. If a grammar has more than one leftmost derivation for a single sequential form, the grammar is ambiguous 
3. If a grammar has more than one rightmost derivation for a single sequential form, the grammar is ambiguous 

> [!Note]
> It is sufficient for one of the above to hold

What do we do then? Rewrite the grammar.

#### Eliminating Ambiguity
![[Pasted image 20230406172103.png]]
Like mentioned above, to deal with ambiguity we need to rewrite the grammar to avoid the problem
![[Pasted image 20230406172137.png]]

#### Parsing Techniques
1. Top-down parser:
	* Construct the top node of the tree and grow towards the leaves, trying to match the input in pre-order (depth first)
	* Pick a production (?) and try to match the input; if you fail then backtrack
	* Essentially, we find a leftmost derivation for the input string (which we need to scan left-to-right)
	* Some grammars are backtrack free (predictive parsing)
2. Bottom-up parser:
	* Construct the tree for an input string, beginning at the leaves and working up towards the top (root)
	* Bottom-up parsing, using again left-to-right scan of the input, tries to construct a rightmost derivation in reverse
	* Can be automated and handle a large class of grammars

### Top-Down Parsing
1. Begin with the starting symbol of the grammar (parse tree root)
2. Repeat until you match (fully) the input string
* Find the leftmost non-terminal symbol in the current sentential form
* Select a production rule with A on it's left hand-side and substitute with the right-hand-side
* Match any terminal symbols to the input
The key is picking the right production rule otherwise the parser will need to backtrack. In principle, choosing the right production rule should be guided by the input string (remember we need an algorithm, we don't want to use out human understanding of the input)
By choosing the right production rule, what I understand is for example choosing `minus`
or `-` as production rule instead of `add` or `+`
![[Pasted image 20230406182419.png]]
Look how in `Steps (one scneario from many)`, we choose `Rule 3` instead of `Rule 2` and why do we do that. But this is how a human would see the parsing as, how would a compiler know what production rule should it take?
#### Left-Recursive Grammars
**Definition**: A grammar is left-recursive if it has a non-terminal symbol $A$, such that there is a derivation $A \implies Aa$ for some string $a$
A left-recursive grammar can cause a top-down parser to go into an infinite loop
**Eliminating left-recursion**: In many cases, it is sufficient to replace $A\implies Aa|b$ with $A \implies bA'$ and $A' \implies aA' | \epsilon$ 
**Example:**
$Sum \implies Sum+number|number$
would become ...
$Sum \implies number \ Sum'$
$Sum' \implies +number \ Sum'|\epsilon$
------------------------------------------------------------------------
![[Pasted image 20230406183216.png]]
------------------------------------------------------------------------------------------------
We can produce a top-down parser but:
* If it picks the wrong production rule it has to backtrack
* For any rule of the form $A \implies B | C$ we want to have a distinct way of choosing the correct production rule to substitute

> [!Idea]
> Look ahead in input and use this to pick correctly 

How much lookahead is required?
* In general, a large amount. Fortunately, most programming languages fall into subclasses of context-free grammars that can be parsed with limited lookahead
The LL(1) property:
* If $A\implies B$ and $A \implies C$ both appear in the grammar, we need to make sure that any first terminal symbol producted by B is different from any first terminal symbol produced by C. This would allow the parser to make a correct choice with a lookahead of exactly one symbol
* In many cases, if the LL(1) property does not exist, we can transform the grammar by factoring any common prefix. Example:
	$A\implies ab_1 | ab_2 | ab_3$ would become $A \implies aZ$ where $Z \implies b_1|b_2|b_3$ 
#### Example
![[Pasted image 20230406184322.png]]
The next symbol determines each choice correctly (check when rules 11,9,4,10,7,11,9,5 are applied in the example). Our grammar is backtrack-free, allowing **predictive top-down parsing** by checking the next symbol of the input

### Bottom-Up Parsing
**Goal:** Given a grammar, $G$, construct a parse tree for a string (i.e. sentence) by starting from the leaves and working to the root (i.e. by working from the input sentence back towards the start symbol $S$)
**Recall:** The point of parsing is to construct a derivation:
$S \implies \delta_0 \implies \delta_1 \implies \delta_2 \implies ... \delta_{n-1} \implies sentence$ 
To derive $\delta_{i-1}$ from $\delta_i$ , we match some right-hand-side $b$ in $\delta_i$ then replace $b$ with with it's corresponding left-hand-side, $A$. This is called **reduction** (it assumes there is a rule $A \implies b$)
##### Example: 
Consider the grammar below and the input string $abbcde$
![[Pasted image 20230406205031.png]]

#### Finding Reductions
Trying to find a substring $b$ that matches the right-hand-side of a production that occurs as one step in the rightmost derivation. Informally, this substring is called a **handle**.
Formally a handle of a right-sentential form $\delta$ is a pair $<A\implies b,k>$ where $A\implies b \in P$ and $k$ is the position in $\delta$ of $b$'s rightmost symbol

> [!Definition]
> Right-sentential form: A sentential form that occurs in some rightmost derivation.

If a grammar is ambiguous, then every right-sentential form has a <u>unique handle</u> 
If we know these handles, we have a derivation
To construct a rightmost derivation, apply this algorithm:
![[Pasted image 20230406205737.png]]

#### Implementing a shift-reduce parser
One implementation makes use of a stack to hold grammar symbols and an input buffer to hold the string to be parsed. Four operations apply:
1. Shift: Next input is shifted (pushed) on top of stack
2. Reduce: Right-end of the handle is on top of the stack, locate left-end of the handle  within the stack; pop handle off stack and push appropriate non-terminal left-hand-side symbol
3. Accept: Parsing ending with success
4. Error: Call an error recovery routine
![[Pasted image 20230406210117.png]]
##### Example
![[Pasted image 20230406210602.png]]
> Would be better to watch his explanation on this (05:36)
> (https://video.manchester.ac.uk/embedded/00000000-011f-6617-0000-0177d6c746c0)

#### What can go wrong?
1. Shift/reduce conflicts: The parser cannot decide whether to shift or to reduce
	Example: The dangling-else grammar; usually due to to ambiguous grammars 
	Solution: a) modify the grammar; b) resolve in favour of a shift
2. Reduce/ reduce conflicts: The parser cannot decide which of several reductions to make
	Example: $id(id,id)$: reduction is dependent on whether the first $id$ refers to an array or a function
	Can also be difficult to tackle
Key to efficient bottom-up parsing: the handle-finding mechanisms

#### LR(1) Grammars

> [!Definition]
> $L$ stands for left-to-right scanning of the input; R for constructing a right-most derivation 
in reverse; 1 for the number of input symbols for lookahead

A grammar is LR(1) if, given a rightmost derivation, we can:
1. Isolate the handle of each right-sentential form
2. Determine the production by which to reduce, by scanning the sentential form from left-to-right, going at most 1 symbol beyond the right-end of the handle
LR(1) grammars are widely used to construct (automatically) efficient and flexible parsers:
* Virtually all context-free programming language constructs can be expressed in an LR(1) form
* LR grammars are the most general grammars parsable by a non-backtracking, shift-reduce parser (deterministic Context-free-grammars)
* Parsers can be implemented in time proportional to tokens + reductions
* LR parsers can detect an error as soon as possible in a left-to-right scan of the input
#### LR Parsing: Background
* Read tokens from an input buffer (same as with shift-reduce parsers)
* Add extra state information after each symbol in the stack. The stack summarises the information contained in the stack below it. For example, the stack could look like:
	$S_0 \ Expr \ S_1 - S_2 \ num \ S_3$ 
* Use a table that consists of two parts:
	* action (stateOnTopOfStack, input_symbol(next)): returns one of: shift (push next symbol and state s); reduce by a rule; accept; error
	* goto (stateOnTopOfStack, non_terminal_symbol): returns a new state to push onto the stack after a reduction
##### Skeleton code for an LR parser
![[Pasted image 20230406212523.png]]
As long as we know the **ACTION** and **GOTO** tables, parsing is simple as running this piece of code

#### The Big Picture
How a compiler does syntax analysis?
![[Pasted image 20230406212740.png]]
* LR(1) parsers are table-driven, shift-reduce parsers that allow automation
* Key: language of handles is regular 
	* Action and goto tables encode the DFA
* How do we generate the Action and Goto tables?
	* Use the grammar to build a model of DFA
* Three commonly used algorithms to build tables: 
	1. LR(1): Full set of LR(1) grammars; large tables; slow, large construction
	2. SLR(1): Smallest class of grammars; smallest tables; simple, fast construction
	3. LALR(1): Intermediate sized set of grammars; smallest tables; very common 

##### Examples:
![[Pasted image 20230406213217.png]]

![[Pasted image 20230406213248.png]]
![[Pasted image 20230406213321.png]]
This is the continuation of the above example

### Summary
![[Pasted image 20230406213426.png]]
