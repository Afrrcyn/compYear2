[[COMP26020]]

# Introduction

Any program written in a programming language must be translated before it can be executed.

This translation is typically accomplished by a software system called compiler.

## Compiler - Definition

A compiler is a program that accepts as input a program text in a certain language and produces an output a program text in another language while preserving the meaning of that text.

A compiler is a program that reads a program written in one language (source language) and translates it into an equivalent program in another language (target language)

## Interpreter - Definition

An interpreter is a program that reads a source program and produces the results of executing the source. It reads the source and produces the result at the same time.

---

A compiler will read the source input and produce the target output for the whole input (typically like a new file), whereas an interpreter will read the source line by line and execute it the same time

## Examples

- C is typically compiled
- Python and PHP are interpreted
- Java is compiled to byte codes, which are then interpreted

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled.png)

## Principles of Compilation

The compiler must:

- Preserve the meaning of the program being compiled
- ‘improve’ the source code in some way - it should try to produce code that runs more efficiently that optimises the source

Other issues: 

- Speed (of compiled code)
- Space (size of compiled code)
- Feedback (information provided to the user)
- Debugging (transformations obscure the relationship source vs target)
- Compilation time efficiency (fast or slow compiler)

## Qualities of a good compiler

- Generates correct code (very important!)
- Generates fast code
- Conforms to the specifications of the input language
- Copes with essentially arbitrary input size, variables, e.t.c
- Compilation time (linearly) proportional to size of source
- Good diagnostics
- Consistent optimisations
- Works well with the debugger

## Uses of Compiler Technology

Most common compilers use: translating a high-level program to object code

- Program translation: Binary translation, hardware synthesis, …

Optimisations for computer architectures:

- Improve program performance, take into account hardware parallelism, e.t.c

Automatic parallelisation or vectorisation

Performance instrumentation: e.g. `-pg` option of `cc` or `gcc`

<aside>
💡 `cc` and `gcc` are both compilers for the C programming language. `cc` is the original C compiler developed by Dennis Ritchie in the early days of Unix, while `gcc` (short for GNU Compiler Collection) is a more modern and widely used compiler that supports a variety of programming languages in addition to C. `gcc` is also open source, which has contributed to its popularity and widespread use.

</aside>

Interpreters:

- E.g. Python, ruby, perl, Matlab, sh, …

Software productivity tools

- Debugging aids: e.g. purify

Security:

- Java VM uses compiler analysis to prove ‘safety’ of Java code

Text-formatters, just in-time compilation for Java, power management, global distributed computing, … 

**Key: Ability to extract properties of a source code (analysis) and transform it to construct a target program (synthesis)**

# Structure of a Compiler

The compiler must:

1. Generate correct code
2. Recognise errors
3. Analyses and synthesises 

## Conceptual Structure

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%201.png)

A compiler has two major phases:

### Front-end

The front-end performs the analysis of the source language.

- Recognises legal and illegal problems and reports errors
- ‘Understands’ the input program and and collects it’s semantics in an IR
- Produces IR and shapes the code for the back-end
- Much of this can just be automated

### Back-end

The back-end does the target language synthesis:

- Chooses instructions to implement each IR (Intermediate Representation) operation

<aside>
💡 IR refers to the representation of the source code that is created by the compiler before it is transformed into executable code. The purpose of the intermediate representation is to make it easier for the compiler to perform optimisations and code generation.

</aside>

- Translates IR into target code Needs to conform with system interfaces
- Automation has been less successful

Typically, front-end usually is $O(n)$ while back-end is NP-complete

<aside>
💡 NP-complete: A problem which we don’t know how to solve in less than exponential time

</aside>

## `m x n` compilers with `m + n` components

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%202.png)

Compiler developers can use different front-ends, different back-ends, and as long as they have the same intermediate representation, then they can combine different front-ends with different back-ends to produce compilers more efficiently 

All language specific knowledge must be encoded in the front-end

All target specific knowledge must be encoded in the back-end

However, in practice, this strict representation is not free of charge, comes with a cost (?)

## General Structure of a Compiler

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%203.png)

**Front-end**

First we have to read the source, the source for the compiler is just a collection of characters. These characters then have to be grouped into words or **tokens** for which **Lexical Analysis** is responsible for. Second phase, we need to ensure the syntax of the code is correct, which is done by **Syntax Analysis.**

Syntax Analysis will produce the abstract syntax tree, which is some form of graph representation, which will be checked for errors related to meaning semantic analysis, for instance, is a variable declared before it is being accessed.

Then, intermediate code, which is quite similar to to the abstract syntax tree, is produced. The IR will be used by the compiler to synthesise the target output. Then, the IR will be optimised. Code generation (further) and further optimisation takes place. 

Order of the front-end is as shown in the graph, however order of the back-end is not really like that as a lot of times, code is re-generated and re-optimised.

## Lexical Analysis (Scanning)

Reads characters in the source program and groups them into words (basic unit of syntax)
Produces words and recognises what sort they are.
The output is called *token* and is a pair of the form `<type, lexeme>` or `<token_class, attribute>`

For example, a = b + c becomes `<id, a> <=,> <id,b> <+,> <id,c>`

> Notice how the `lexeme` or `attribute` is empty for mathematical symbols

Needs to record each id attribute: keep a **symbol table**

Lexical analysis removes white spaces, e.t.c. (does it also remove `\n` ?)

Speed is of the crucial here, use a specialised tool: e.g. `flex` - a tool for generating scanners: programs which recognise lexical patterns in text;

```c
% man flex
```

## Syntax (Syntactic) Analysis (Parsing)

Imposes a hierarchical structure on the token stream

This hierarchical structure is usually expressed by recursive rules

Context-free grammars formalise these recursive rules and guide syntax analysis

Example:

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%204.png)

## Parsing

The output of syntax analysis is a parse tree

This tree puts some hierarchy into expressions.

Leaves of the tree will be the elements of the input, which will be `b * b - 4 * a - c` (individual?). The rest of the tree are the symbols of the language which are necessary in order to recognise that this input is valid. Because we can produce this tree, we know this input is valid

The following is a parsing example of a parse tree for `b * b - 4 * a - c`

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%205.png)

## Abstract Syntax Tree (AST)

An abstract syntax tree (AST) is a more useful data structure for internal representation. It is a compressed version of the parse tree (summary of grammatical structure without details about it’s derivation)

ASTs are one form of IR

Once we terminate the syntax analysis, we get an abstract syntax tree. We get rid of the extra symbol we introduced in order to find out that the expression is syntactically correct.

The following is an AST for:  `b * b - 4 * a * c`

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%206.png)

## Semantic Analysis (Context Handling)

Once done with the AST, the last part for the front-end is to check for semantic errors.

Semantic analysis collects context (semantic) information, checks for semantic errors, and annotates nodes of the tree with results

Examples:

- Type checking: Report error if an operator is applied to an incompatible operand (for example addition of a string to an integer)
- Check if a variable has been properly declared
- Name-related checks

Once all of that is done, the compiler can move on to the back-end compilation as there were no errors detected on the front-end (no syntactical or semantic errors)

## Intermediate Representation

Translate language-specific constructs in the AST into more general constructs

A criterion for the level of ‘generality’: it should be straightforward to generate the target code from the intermediate representation chosen.

Example of a form of IR (3-address code) for `b * b - 4 * a * c`:

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%207.png)

Note: 3-address code means one operand and at-most three operators
 
```c
<operand> = <operator> + <operator>
```

This representation is very closely related to low-level assembly code, which is why this form of representation may be good.

## Code Optimisation

The goal is to improve the intermediate code and, thus, the effectiveness of code generation and the performance of the target code.

Optimisations can vary from trivial (e.g. constant folding) to highly sophisticated (e.g. in-lining)

For example:

```c
tmp1 = 4
tmp2 = tmp1 * a
// all of that can be replaced with:
tmp1 = 4 * a
```

Modern compilers perform such a range of optimisations, that one could argue for:

![Middle-end optimiser between front-end and back-end](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%208.png)

Middle-end optimiser between front-end and back-end

## Code Generation Phase

After optimisation, there is code generation phase.

Map the AST onto a linear list of target machine instructions in a symbolic form.

- Instruction selection: A pattern matching problem
- Register allocation: Each value should be in a register when it is used (but there is only a limited number): NP-complete problem. We use registers because registers are faster compared to taking data from memory
- Instruction scheduling: Take advantage of multiple functional units: NP-Complete problem

Target, machine-specific properties may be used to optimise the code

Finally, machine code and associated information required by the operating system are generated

## History

Unexamined part?

![Untitled](Introduction%20to%20Compilation%2050d25506368e443b9aa55d08b53c0109/Untitled%209.png)

## Summary

A compiler is a program that converts some input text in a source language to output in a target language

Compiler construction process poses some of the most challenging problems in computer science

Compilers operate in phases:

- The key principles of each phase will be described next
- Parts of a compiler can be generated automatically using generators based on formalisms. Example flex (scanner generator), bison (parse generator)