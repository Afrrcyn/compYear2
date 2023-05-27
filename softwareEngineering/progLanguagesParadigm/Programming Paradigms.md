[[COMP26020]]

A programming paradigm defines a fundamental style of programming

- How the programmer can describe the program in the source code:
    - the data used by the program, and the computations manipulating the data

Programming paradigms can be used to classify programming languages

Each programming language belongs to (or implements) one or more programming paradigms, which makes it easy to develop using the style defined by the paradigm

- Diagram of Programming Paradigms and Languages
    
    ![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled.png)
    
    From the diagram we can see that C++ and C# belong to Object Oriented paradigm, and so on. We can see that OCaml belongs to more than one paradigms, object oriented and functional.
    

Some paradigms are better suited than others to solve a given kind of software engineering problem

- Paradigm Example
    **Example**: Multiplying each element of an array by 2 in JavaScript
    1. Using ==Imperative== Paradigm
        
        <aside>
        💡 Imperative programming paradigm: Works by changing program state through assignment statements. Performs step by step task by changing state
        
        </aside>
        
        ```jsx
        function mult_by_two_imperative(array){
        	let results = []
        	for (let i = 0; i < array.length; i++){
        		results.push(array[i]*2)
        	}
        	return results
        }
        
        array = [1,2,3]
        res = mult_by_two_imperative(array)
        console.log(`Result: ${res}`);
        ```
        
    2. Using ==Declarative== Paradigm
        <aside>
        💡 Style of building programs that expresses logic of computation without talking about its control flow.
        
        </aside>
        
        ```jsx
		function mult_by_two_declarative (array) {
			return array.map((item) => item*2)
		}
		
		array = [1,2,3]
		res = mult_by_two_declarative(array)
		console.log(`Result: ${res}`);
        ```
    
    Results are the same despite the difference in describing computations
    
    As seen that Declarative paradigm takes fewer lines of codes than Imperative, we can say that Declarative paradigm is more efficient and elegant, even though it can be a little harder to understand.


Paradigm defines how the programmer describes the program computations

- Can be sequences of statements, executed sequentially or in parallel
- Can also be how the result should look like
- Can be divided into functions

Paradigm also defines how the programmer descries the data computations are applied on:

- Basic types
- Custom data structures
- Local / global variables
- etc

##### How to pick a programming paradigm?

Choosing a language to solve a given software engineering problem implicitly selects one (or more) paradigm(s)

This has huge consequences on how to design the solution and map it to the code, which can impact the efficiency, size, complexity and clarity of the code

## The Main Programming Paradigms

High Level Programming Paradigms and Sub-Paradigms

1. Imperative
    1. Structured (or procedural)
    2. Object Oriented
    3. Concurrent
2. Declarative
    1. Functional
    2. Concurrent

- Imperative Programming Paradigm
    
    Programmer describes sequences of statements manipulating the program state, basically like a cooking recipe
    e.g. describe how to obtain the computation results
    Assembly language is imperative even though it is a low level language. It has very low memory footprints (which is an advantage)
    
    Paradigms:
    1. Structured Programming
        Programmer uses advanced **control flow operations:**
        - Loops, conditionals, procedures
        - Compared to pure imperative, easier to describe/reason about complex/large programs
    2. Object Oriented Programming
        - Encapsulation of code and the associated data into objects
        - Inheritance and polymorphism
            
            ```jsx
            data.operation() // dot operator to access attributes
            ```
            
        - Well suited to represent problems with a lot of state / operations
            - GUI, simulators, video games, business management software, and many other use cases
        - Ease reasoning about / organizing large codebases
        - Can sometimes be overkill for small programs
    3. Concurrent Programming
        - Can use threads/processes to describe interleaving and/or parallel computation flows
            ![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled%201.png)
        - Many imperative languages provide ways to exploit concurrency:
            - Shared-memory threads / processes in C/C++, Java, Python
            - Message passing (MPI) in C/C++/FORTRAN
            - Semi-automatic loop parallelization with libraries such as OpenMP
            - GPU programming with CUDA
        - Use cases: HPC, distributed computing, graphic processing, etc
- Declarative Programming Paradigm
    The programmer describes the meaning/result of computations
    ![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled%202.png)
    
    Declarative Programming Paradigm has:
    - High level of abstraction
    - Code can easily become convoluted
        <aside>
        💡 Convoluted: Very complicated or involved with many details, complex.
        
        </aside>
        
    - Languages: HTML, SQL, XML, CSS, Latex
    - Usage: document rendering, structured data storage and manipulation
    
    Paradigms:
    1. Functional
        - Calling and composing functions to describe the program
            ![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled%203.png)
            
        - First-class / higher-order functions
        - Loops implemented with recursion
        - Pure functions have no side-effects
        - Languages: Haskell, Scala, F#, etc
    2. Concurrent
        
        ![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled%204.png)
        
        - Less need for synchronization
        - Use cases: distributed applications, web services, etc
- Other programming paradigms
    - Logic
    - Dataflow
    - Metaprogramming/Reflexive
    - Constraint
    - Aspect-oriented
    - Quantum
- Multi-paradigm Languages
    - Haskell: Purely functional and does not allow Object Oriented
    - OCaml: Mainly functional, allows Object Oriented and imperative constructs
    - C, C++: Imperative but allow some functional idoms, for example
        
        ![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled%205.png)
        
    - Many languages are multi-paradigm
    

![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled%206.png)

Summary:

![Untitled](Week%201%2039bb2480db9342e2b1f84f70dd2c5b21/Untitled%207.png)