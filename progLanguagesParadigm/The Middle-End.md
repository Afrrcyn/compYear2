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




