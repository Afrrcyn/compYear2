[[COMP26020]]

Lexical Analysis, also known as ==scanning== 

### Introduction
![[Pasted image 20230405010044.png]]
Lexical analysis reads characters and produces sequences of tokens. It recognises the language's part of speech.
In <u>natural languages</u>, mapping words to part of speech is called idiosyncratic 
In <u>formal languages</u>, mapping words to part of speech is syntactic

> Idiosyncratic: Unique to a particular individual or group. When we say that mapping words to part of speech in natural languages is idiosyncratic, we mean that it can vary depending on the language, dialect, context, and even the speaker's style or preference.

#### Some Definitions
* A vocabulary (alphabet) is a finite set of symbols
* A string is a finite sequence of symbols from a vocabulary
* A language is a set of strings over a fixed voacbulary
* A grammar is a finite way of describing a language
* A context-free grammar, $G$ is a 4-tuple $G = (S,N,T,P)$ where:
					$S$ = starting symbol
					$N$ = Set of non-terminal symbols
					$T$ = Set of terminal symbols
					$P$ = Set of production symbols
A language is the set of all terminal productions of $G$
##### Example
![[Pasted image 20230405011147.png]]
##### Another Example - Left most derivation
![[Pasted image 20230405011639.png]]
This example is known as left-most derivation, why? Because we are replacing the left most variable and substituting with a possible value, at each step. The final outcome is a possible outcome from the given grammar. This wil be later needed at [[Syntax Analysis]]

> [!Note]
> Wherever possible, it is common for non-terminal symbols to start with an upper-case letter.

### Why is all of this needed?
#### Why study lexical analysis?
- To avoid writing lexical analysers (scanners) by hand
- To simplify specification and implementation
- To understand the underlying techniques and technologies
We want to specify **lexical patterns** to derive tokens:
* Some parts are easy for the compiler:
	* White spaces -> blank | tab | combination of blanks and tabs
	* Operators (+, -, *, /)
	* Comments (/$*$ followed by $*$/ in C, or // in C++)
* Other parts are complex:
	* Identifiers (letter followed by alphanumeric characters)
	* Numbers(integers, floating point...)
> We need a notation that would lead to an implementation

### Regular Expressions (RE)
Patterns from a regular language. A regular expression is a way of specifying a regular language. It is a formula that describes a possibly infinite set of strings.

> [!Example]
> `ls [x-z]*` in linux would give you the name of all files starting from `x` to `z`. The number of files it represents would not have a symbol because of the use of `*` operator

Regular expression (RE) (over a vocabulary $V$):
* $\epsilon$ is a RE denoting the empty set $\{\epsilon\}$
* If $a \in V$ then $a$ is a RE denoting $\{a\}$
* If $r_1, r_2$ are REs then:
	* $r_1^*$ denotes zero or more occurrences of $r_1$
	* $r_1r_2$ denotes concatenation of the two regular expressions
	* $r_1|r_2$ denotes either $r_1$ or $r_2$
Shorthands:
* $[a-d]$ for $a|b|c|d$
* $r^+$ for $rr^*$ , which is atleast one r
* $r?$ for $r|\epsilon$
##### Example
![[Pasted image 20230405014516.png]]

Regular expressions specify patterns.
> [!Note]
> Not all languages can be described by regular expressions, but we don't care about this for lexical analysis.

#### Building a Lexical Analyser by Hand
![[Pasted image 20230405014822.png]]

Check if end of file is not reached, then check if input is a letter, if yes then recognise it. Else check if digit, or check if operator, if yes then recognise it, otherwise return error and do the same for all characters. 
This process can be efficient but requires a lot of work and can be difficult to modify
> We want some way of optimising lexical analysis

#### Automating Lexical Analysis
Consider the problem of recognising register names, which start with the letter $r$ and have at least one digit:
$Register -> r(0|1|2|...|9)(0|1|2|...|9)^*$ (or $Register -> r Digit Digit^*)$
The RE corresponds to a transition diagram
![[Pasted image 20230405015317.png]]

The transition diagram depicts the actions that take place by the lexical analyser. When it starts, it has to first recognise an r and then one or more digits. Then it can keep expecting digits or terminate the program.
* A circle represents a state; S0: start state; S2: final state (double circle)
* An arrow represents a transition; the label specifies the cause of the transition
* A string is accepted if, going through the transitions, ends in a final state (for example, r345, r0, r29 as opposed to just a, r, rab)

#### Towards Automation
An easy (computerised) implementation of a transition diagram is a transition table: a column for each input symbol and a row for each state. Each table entry shows a set of states that can be reached from a given state on some input symbol. Example:
![[Pasted image 20230405015849.png]]

Where the table works exactly like the transition diagram above.
If we know the transition table and the final state(s) then building a lexical analyser should be easy
![[Pasted image 20230405015955.png]]

#### Finite Automaton
The generalised transition diagram is a finite automaton. It can be:
* Deterministic, DFA: As in the above examples, for every state and symbol there is only **one** transition to another state (deterministic)
* Non-deterministic, NFA: More than one transition out of a state may be possible on the same input symbol: think about: $(a|b)^* abb$
	![[Pasted image 20230405020309.png]]
	Look at $S_1$, it is evident that it can loop over itself or go on to $S_2$ hence there is no one transition to another state, therefore non-deterministic x

##### Recap
* Regular expressions (REs) are formulae to describe a (regular) language
* Every RE can be converted to a deterministic finite automation (DFA) (typically through an NFA first (?))
* DFAs can automate the construction of lexical analysers

### Automatic Lexical Analysis Construction
To convert a specification into code:
* Write down the regular expression for the input language
* Convert the regular expression into a NFA (Thompson's construction)
* Build the DFA that stimulates the NFA (subset construction)
* Minimise the DFA (Hopcroft's algorithm)

> [!Note]
> There is a full cycle, DFA to RE construction is all pairs, all paths. Which means we can convert the DFA back into a regular expression if we wanted to.

##### Lexical Analyser Generators (Scanners)
* Software tools such as lex or flex work along these lines
* Algorithms are well known and understood

#### RE to NFA using Thompson's Construction
Key idea: NFA pattern for each symbol and/or operator: join them in precedence order
![[Pasted image 20230405065916.png]]
> [!Note]
> Remember that $\epsilon$ denotes an empty symbol

##### Example: Build the NFA of $a(b|c)^*$
![[Pasted image 20230405070041.png]]

#### NFA to DFA: Two key functions
1. $move(s_i, a)$: the (union of the) set of states to which there is a transition on input symbol a from state $s_i$
2. $\epsilon-closure(s_i)$: the (union of the) set of states reachable by $\epsilon$ from $s_i$
Example (see below):
![[Pasted image 20230405070711.png]]
Try understanding the diagram carefully:
So, look at $\epsilon-closure(3)$, where 3 is our starting state or $s_i$. How far can you go with $\epsilon$ with starting point at 3? we can go from 3 to 4 to 7 and that is it. Hence $\epsilon-closure(3) = \{3,4,7\}$
Similarly for 10, we get $\epsilon-closure(3) = \{3,4,7, 10\}$
Next, for $move(\{3,10\}, a)$, we start from {3,10} moving it to where we see an $a$. We see an $a$ that leads us to 8, hence there we get our move function.

The algorithm (subset construction):
* Start with the $\epsilon - closure$ of the starting state from NFA 
* Repeat for the set of states you find and any new one:
	* For each symbol, take their  $\epsilon - closure(move(state,symbol))$

##### Example: NFA for $(a|b)^*abb$
![[Pasted image 20230405071503.png]]
> Not very hard to understand, would not add how moves are calculated because straightforward. If do not understand then watch the video.
> What do we get from this?

##### Result of applying subset construction
Basically converting NFA to DFA now, as the nodes only have a single path
![[Pasted image 20230405071718.png]]
States with destinations if they take $a$ or $b$.
This is now a DFA

### DFA Minimisation: The problem
![[Pasted image 20230405072005.png]]
So now we want to see if we can minimise the number of states.
We can indeed minimise number of states, if we find groups of states where, for each input symbol, every state of a group will have transitions to the same group.
One solution would be to start with two groups. Final vs non-final states, and check if this property holds; if does not, then split and keep checking  (Hopcraft's algorithm)

![[Pasted image 20230405072355.png]]
Separate states by if they are terminating states or not. If non-terminating states are going outside the group they were in, separate those states, and check within the group again.

#### From NFA to Minimised DFA $a(b|c)^*$
![[Pasted image 20230405072528.png]]

This is how it would be minimised as a DFA. Split states into terminating and non-terminating, and then find if states going out of current group. Here $a$ was pointed to the other group. What if the other group was not a terminating group?
![[Pasted image 20230405072756.png]]

### Building Fast Scanners
Why have we been converting a RE into a DFA?
If we know the transition table and the final state(s) we can build directly a recogniser that detects acceptance
![[Pasted image 20230405224914.png]]
However, table-driven recognisers can waste a lot of effort, why? In reality, programming languages will generate very large tables. So in this process of generating a table is captured into code directly. The whole table is directed into the code
##### How are reserved keywords dealt with?
* Many compilers recognise keywords as identifiers and check them in a table
* One may also include them as REs in the lexical analyser's specification

#### Direct coding from the transition table
> [!Recall]
> $Regisiter -> r \ digit \ digit^*$

The following bundles together transition table and code:
![[Pasted image 20230405225552.png]]
This ensures:
* Fewer operations, hence runs faster
* Avoids memory operations (especially important for large tables)
* Added complexity may make the code ugly and difficult to understand, however it is all generated automatically.
#### Practical Considerations
Poor language design may complicate lexical analysis:
* `if if then then = else else else = then (PL/I)` - I do not know what this means?
* `DO5I = 1,25` vs `DO5I = 1.25` - here, the first assignment is a range value from 1 to 25 till 5 is detected, the second assignment is just assigning the double value `1.25` to a variable called `DO51`
* The development of a sound theoretical basis has influenced language design positively. Both examples are from languages that pre-date lexical analysis as we know it today.
Template syntax in C++:
* `aaaa<mytype>`
* `aaaa<mytype<int>>` (where `>>` is an operator for writing to the output stream ...)
The lexical analyser treates the `>>` operator as two consecutive `>` symbols. The confusion will be resolved by the parser (by matching `<,>`)

#### Flex / Lex: Generating Lexical Analyser
**Flex** is a tool for generating lexical analysers: programs which recognise lexical patterns in text (from the online manual pages: `% man flex`)
The input consists of 3 sections:
* Regular expressions
* Pairs of regular expressions and C code
* Auxiliary C code
When the input is compiled, it generates as output a C source file `lex.yy.c` that contains a routine `yylex()`. After compiling the C file, the executable will start isolating tokens from the input according to the regular expressions, and, for each token, will execute the code associated with it. The array char `yytext[]` contains the representation of a token.
##### Flex Example
![[Pasted image 20230405235113.png]]
From this example, have a look at the input file and the output of it. You can see that for each input, there is a corresponding value in the output section. For every pattern not defined, it returns `-1` which is defined for `ERROR`
Therefore, the `flex` software can produce tokens easily without the compiler developers about lexical analysis.

### Summary
Lexical analysis turns a stream of characters into a stream of tokens:
* an automated process: from REs to NFAs to DFAs
* REs are powerful enough to specify patterns and allow us to automate the process