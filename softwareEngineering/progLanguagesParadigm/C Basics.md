[[COMP26020]]

[Build software better, together](https://sandbox.cs50.io/af21709d-261a-4f04-b799-eee235a5e510)

Sandbox

## Introduction to C (Slide 1)

- Characteristics of C
    
    Pros:
    
    - Syntax
    - Low level language
    - Fast
    - Controlled memory footprint
    
    Cons:
    
    - Large area for making mistakes
    - Lack of memory safety, risks of undefined behaviour at runtime
    
    Used in systems software, high performance computing, embedded systems
    
    - Some software written in C
        1. Linux
        2. iOS
        3. git
        4. Apache
    

### Hello World code

```c
#include <stdio.h>
int main() {
	printf ("hello, world!\n");
	return 0;
}
```

- `#include` : get access to functions from external libraries
- `int`: function needs to return an `integer` at the end of execution, hence we return 0
    
    <aside>
    💡 We can also choose to use `void main()` and not return any integer at the end of execution, however usually returns with a warning so should be avoided
    
    </aside>
    
- `<stdio.h>` : standard input output library in C
- `printf` : used to print text on console as output
- `return` : to go back to calling context, AND exit if returning from `main`

### Compilation

![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled.png)

```c
$ gcc listing1.c -o listing1

gcc: GNU Compiler Collection (compiler)
listing1.c : name of C file
listing1 : name of output executable file which is a binary file
```

### Warnings and Errors

- Errors are not unrecoverable
- Warning may indicate a problem
    
    ![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%201.png)
    
- All warnings should be fixed
- Some warnings (picky ones) are disabled by default, enable them with the `-Wall -pedantic` flags

### Comments

![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%202.png)

Use comments to explain what the code is doing

## Variables, Types and Printing to the Console (Slide 2)

### Variables

```c
#include <stdio.h>
int main() {
    int a;                          // declare a of type int (signed integer)
    a = 12;                         // set the value of a to 12
    printf("a's value: %d\n", a);   // print the value of a
    return 0;
}

%d: one of the format specifiers, %d means signed decimal integer
%i: format specifier for integer

/*
a in the upper example can also be declared and initalised as
*/
int a = 12;
```

Variables must be declared before being used, to declare one:

`<variable type> <variable name>`

### Rules for naming variables

1. Variables name should start with a letter or an underscore (like python)
    
    `_number` or `number`
    
2. It can contain a combination of letters, numbers or underscores
    
    `final_number2`
    
3. Names should be meaningful and not very long

### Other code functions

```c
int a; int b; int c; // can declare multiple variables on the same line
int d = 12;  // declare and set (initialise) on the same line
int x, y = 10, z = 11;
a = 12;      // set a to 12
b = 20;      // set b to 20

/*
We can't be certain for the value of x in this case, it can either be null
which is 0 for integers, or it could a random number allocated to it on memory
however if we remove %d from the output, it gives us a blank output
*/

c = 10 + 10; // set c to 20
a = b;       // a = 20
d++;         // d = d + 1
y *= 2       // y = y * 2;
```

### Types

- At build time, compiler checks the validity of operations on the variable based on its type
- The type also defines the amount of space in memory allocated to the variable

Basic types of categories:

1. Integers
2. Floating-point numbers
3. Characters

```c
int a;
a = -12345

// signed integer takes 4 bytes in memory on the Intel x86-64 architecture
```

![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%203.png)

### Characters and float

```c
char my_char_variable;
float my_float_variable;

my_char_variable = 'x';   // use single quotes for characters
my_float_variable = 12.4;

printf("the character value is %c \n", my_char_variable);

// don't forget to add %c as format specifier
```

### Arithmetic mixing integers and floats

```c
int int1 = 2, int2 = 4;
float float1 = 2.8;
/* when mixed with floats in arithmetics, integers are promoted to floats */
float float_res = int1 + float1;
float float_res2 = int1 * (float1 + int2);
/* when stored in an integer variable, floats are _truncated_ */
int int_res = int1 * (float1 + int2);

printf("float result:   %f\n", float_res);
>>> 4.800000
printf("float result2:   %f\n", float_res2);
>>> 13.600000
printf("integer result: %d\n", int_res);
>>> 13
```

### More Types, Qualifiers

[C data types - Wikipedia](https://en.wikipedia.org/wiki/C_data_types)

Types: `char`, `int`, `float`, `double`

Qualifiers: `signed`, `unsigned`, `short`, `long`

Determines the storage size in memory

Exact implementation is architecture dependent 

```c
short int a;              // signed, at least 16 bits: [−32,767,      +32,767]
int b;                    // signed, at least 16 bits: [−32,767,      +32,767]
unsigned int c;           // unsigned:                 [0,            +65,535]
long int d;               // at least 32 bits:         [−2,147,483,647,+2,147,483,647]
unsigned long int e;      // unsigned:                 [0,     +4,294,967,295]
long long int f;          // at least 64 bits:         [-9x10^18,    +9x10^18]
long long unsigned int g; // unsigned:                 [0,          +18x10^18]
float h;                  // storage size unspecified, generally 32 bits
double i;                 // storage size unspecified, generally 64 bits
```

The function `sizeof()` can be used to get the exact size of a type on a given machine

```c
int so_short = sizeof(short int);
int so_int = sizeof(int);
int so_uint = sizeof(unsigned int);
int so_long = sizeof(long int);
int so_longlong = sizeof(long long int);
int so_float = sizeof(float);
int so_double = sizeof(double);
    
printf("size of short:         %d bytes\n", so_short);
printf("size of int:           %d bytes\n", so_int);
printf("size of unsigned int:  %d bytes\n", so_uint);
printf("size of long int:      %d bytes\n", so_long);
printf("size of long long int: %d bytes\n", so_longlong);
printf("size of float:         %d bytes\n", so_float);
printf("size of double:        %d bytes\n", so_double);
```

![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%204.png)

### Printing to the Console

Accepts one or more arguments:

- format string
- optionally a list of variables which values should be printed, replacing markers (e.g. `%d`in format string)

[https://linux.die.net/man/3/printf](https://linux.die.net/man/3/printf)

 

```c
#include <stdio.h>

int main() {

    int int_var = -1;
    unsigned int uint_var = 12;
    long int lint_var = 10;
    float float_var = 2.5;
    double double_var = 2.5;
    char char_var = 'a';
    char string_var[] = "hello";

    printf("Integer: %d\n", int_var);
    printf("Unsigned integer: %u\n", uint_var);
    printf("Long integer: %ld\n", lint_var);
    printf("Float: %f\n", float_var);
    printf("Double: %lf\n", double_var);
    printf("Characters: %c\n", char_var);
    printf("String: %s\n", string_var);
    printf("Several variables: %d, %lf, %s\n", int_var, double_var, string_var);

    return 0;
}

>>>
Integer: -1
Unsigned integer: 12
Long integer: 10
Float: 2.500000
Double: 2.500000
Characters: a
String: hello
Several variables: -1, 2.500000, hello
```

Format string accepts escaped characters such as `\n` for line break

Make sure to use the right marker

- In most situations, the compiler does not tell if a mistake is made and incorrect values will be displayed at runtime
    
    ```c
    		int my_variable = -42;
        printf("-42 printed with %%d: %d\n", my_variable);
        printf("-42 printed with %%u: %u\n", my_variable);  // -42 interpreted as an unsigned integer
        unsigned long long int ull = 5467513055454315;
        printf("5467513055454315 printed with %%llu: %llu\n", ull);
        printf("5467513055454315 printed with %%u: %u\n", ull);// interprets only 4 bytes vs. 8 for llu
        printf("5467513055454315 printed with %%d: %d\n", ull);// first 4 bytes, as unsigned
    >>> 
    -42 printed with %d: -42
    -42 printed with %u: 4294967254
    5467513055454315 printed with %llu: 5467513055454315
    5467513055454315 printed with %u: 2507777131
    5467513055454315 printed with %d: -1787190165
    ```
    

## **Arrays, Strings and Command Line Arguments (Video 1)**

Arrays are used to store multiple values in a single variable, instead of declaring separate variables for each value

Arrays are stored contiguously in memory

<aside>
💡 Contiguous Allocation: Memory is allocated as a single chunk. This is most often used when talking about containers.

</aside>

![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%205.png)

```c
int array[4]; //declaration

array[0] = 10; // writing in slots
array[1] = 20;
array[2] = 30;
array[3] = 40;

int x = array[3]; // reading a slot

printf("array[%d] contains %d\n", 0, array[0]);
printf("array[%d] contains %d\n", 1, array[1]);
printf("array[%d] contains %d\n", 2, array[2]);
printf("array[%d] contains %d\n", 3, array[3]);

>>>
array[0] contains 10
array[1] contains 20
array[2] contains 30
array[3] contains 40
```

### Multi-dimensional Array

Multi-dimensional arrays are also stored contiguously in memory

![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%206.png)

```c
				int my_2d_array[3][2];

        my_2d_array[0][0] = 0;
        my_2d_array[0][1] = 1;
        my_2d_array[1][0] = 2;
        my_2d_array[1][1] = 3;
        my_2d_array[2][0] = 4;
        my_2d_array[2][1] = 5;

        printf("my_2d_array[%d][%d] = %d\n", 0, 0, my_2d_array[0][0]);
        printf("my_2d_array[%d][%d] = %d\n", 0, 1, my_2d_array[0][1]);
        printf("my_2d_array[%d][%d] = %d\n", 1, 0, my_2d_array[1][0]);
        printf("my_2d_array[%d][%d] = %d\n", 1, 1, my_2d_array[1][1]);
        printf("my_2d_array[%d][%d] = %d\n", 2, 0, my_2d_array[2][0]);
        printf("my_2d_array[%d][%d] = %d\n", 2, 1, my_2d_array[2][1]);
```

Static initialisation allows to omit the size (of the first dimension)

```c
int array[] = {1, 2, 3, 4, 5};
int array_2d[][2] = { {1, 2}, {3, 4}, {5, 6} };
printf("array[4] = %d\n", array[4]);
printf("array_2d[2][0] = %d\n", array_2d[2][0]);

>>>
array[4] = 5
array_2d[2][0] = 5
```

### Character Arrays (Strings)

String in C:

Array of characters that ends with the special `\0` end of string character marker, which means length of string will always be one more than it usually is

![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%207.png)

```c
				char string1[6] = "hello";
        char string2[] = "hello";
        char string3[6];

        string3[0] = 'h';
        string3[1] = 'e';
        string3[2] = 'l';
        string3[3] = 'l';
        string3[4] = 'o';
        string3[5] = '\0'; // Important here

        printf("%s\n", string1);
        printf("%s\n", string2);
        printf("%s\n", string3);

>>>
hello
hello
hello
```

### Command Line Arguments

```c
gcc test.c -o test

gcc: program
test.c -o test: arguments

```

- `main` parameters
    - `argc` - integers
    - `argv` - array containing the command line arguments
    
    ```c
    int main(int argc, char **argv) { // 'char ** argv' means 'char argv[][]'
    
        printf("Number of command line arguments: %d\n", argc);
        // This is a bit dangerous...
        printf("argument 0: %s\n", argv[0]);
        printf("argument 1: %s\n", argv[1]);
        printf("argument 2: %s\n", argv[2]);
    
    		return 0;
    }
    >>> listing21.c ali lakho
    Number of command line arguments: 3
    argument 0: ./listing21
    argument 1: ali
    argument 2: lakho
    ```
    
- Arguments are strings, to convert to **integer**, we can use `atoi`
    
    ```c
    #include <stdlib.h> // required for atoi
    /* Sum the two integers passed as command line integers */
    int main(int argc, char **argv) { // same like the one before
        int a, b;
        // Once again, dangerous!
        // We'll fix that in the next lecture regarding control flow
        a = atoi(argv[1]);
        b = atoi(argv[2]);
        printf("%d + %d = %d\n", a, b, a+b);
        return 0;
    }
    >>> ./listing1 5 34
    5 + 34 = 39
    ```
    
- To convert a **string** to a **double, use `atof`**

## Conditionals and Loops (Video 2)

### Control Flow

Statements end with `;` and are executed sequentially, in an order

```c
#include <stdio.h>
int main() {
    printf("hello!\n");     // print hello first
    printf("goodbye!\n");   // then print goodbye
    return 0;               // then return
}
>>>
hello!
goodbye!
```

### Conditionals

```c
if (x == 50) {
    printf("The value of x is 50\n");
}

if (x == 50) {
    printf("The value of x is 50\n");
} else {
    printf("The value of x is not 50\n");
}

if(x < 50) {
    printf("The value of x is strictly less than 50\n");
} else if (x == 50) {
    printf("The value of x is exactly 50\n");
} else {
    printf("The value of x is strictly more than 50\n");
}
```

- **Conditions: `<`, `<=`, `>`, `>=`, `==` (equality), `!=` (difference)**
- `=` is not used for equality as this is an assignment operator
- “false” is 0, “true” is everything but 0
- Boolean Operators:
    - `&&` Logical AND
    - `||` Logical OR
    - `!` Negation
    
    Operators precedence:
    
    - `!` first, then `&&` , then `||`
    - Use parentheses to modify/clarify evaluation order

### Switch Statement

- Test the equality of an integer against a list of values
    
    ```c
    switch(x) {
        case 1:
            printf("x is 1\n");
            break;
        case 2:
            printf("x is 2\n");
            break;
        case 3:
            printf("x is 3\n");
            break;
        default:
            printf("x is neither 1, nor 2, nor 3\n");
    ```
    
- Use `break` to avoid fall through in cases below
- `default` path taken when none of the other cases match

### While Loop

Keep executing loop body while condition is true

`while...` checks the condition before entering the loop body and `do...while` checks after the first iteration

```c
//While loop

int x = -1;
printf("------ while loop:\n"); 
while (x > 0) { // won't enter loop body ever because -1 is not > 0
    printf("x is %d\n", x);
    x = x - 1;
}

// Do while

x = 10;
printf("------ do ... while loop:\n");
do {
    printf("x is %d\n", x);
    x = x - 1;
} while (x > 0); // will do just one iteration as the condition is checked after the
								// loop body
```

Infinite Loop

```c
while (1) {
	printf("hello\n");
}
// this will print hello infinitely many times
```

To kill an infinite program we can use `Ctrl+C`

### For Loops

`for (<init stmt> ; <cond> ; <update stmt>) {<body>}`

- `init stmt` executed once before the first iteration
- `cond` checked before each iteration
- `update stmt` executed after each iteration

```c
		int i;
    for(i = 0; i<10; i = i + 1) {
        printf("i is %d\n", i);
    }
    /* we can declare the iterator as part of the loop header */
    for(int j = 0; j<10; j = j + 1) {
        printf("j is %d\n", j);
    }
```

`break` within a loop simply exits the loop

```c
for(int i = 0; i < 10; i = i + 1) {
        printf("iteration %d\n", i);
        if(i == 5) {
            break;
        }
    }
// will print upto 5 and then stop
```

`continue` within a loop directly goes to the next iteration, in simpler words, skips the iteration

```c
int i = 0;
    while(i < 10) {
        i = i + 1;
        if(i == 5) {
            continue;
        }
        printf("iteration %d\n", i);
    }
```

### Loops and Arrays

```c
int my_array[4]; // 1D Array
for(int i=0; i<4; i = i +1) {
    my_array[i] = 100 + i;
    printf("index %d contains: %d\n", i, my_array[i]);
}
>>>
index 0 contains: 100
index 1 contains: 101
index 2 contains: 102
index 3 contains: 103

int my_2d_array[3][2]; 
for(int i=0; i<3; i++) { // 2D array
    for(int j=0; j<2; j++) {
        my_2d_array[i][j] = i*j;
        printf("Index [%d][%d] = %d\n", i, j, my_2d_array[i][j]);
    }
}
>>> 
Index [0][0] = 0
Index [0][1] = 0
Index [1][0] = 0
Index [1][1] = 1
Index [2][0] = 0
Index [2][1] = 2
```

### Command Line Arguments

```c
#include <stdio.h>
int main(int argc, char **argv) {
    printf("Number of command line arguments: %d\n", argc);
    for(int i=0; i<argc; i++)
        printf("argument %d: %s\n", i, argv[i]);
    return 0;
}
>>> ./listing21 hello arguments
Number of command line arguments: 3
argument 0: ./listing21
argument 1: hello
argument 2: arguments
```

Require user to input set number of arguments

The following code can be used to require the user to input the number of arguments the code wants, in this case, 2

```c
#include <stdio.h>
#include <stdlib.h>

int print_help_and_exit(char **argv) {
    printf("Usage: %s <number 1> <number 2>\n", argv[0]);
    exit(-1);
}
/* Sum the two integers passed as command line integers */
int main(int argc, char **argv) {
    int a, b;
    if(argc != 3)
        print_help_and_exit(argv);
    a = atoi(argv[1]);
    b = atoi(argv[2]);
    printf("%d + %d = %d\n", a, b, a+b);
    return 0;
}

// If user enters 1 argument, an error message will be given saying 
// Usage: ./listing21 <number 1> <number 2>

// Otherwise if 2 arguments (integers) are given here, then the code will
// not give any errors
```

### Braces in Conditionals and Loops

- Conditional / loop body is a single statement? Can omit braces
    
    ```c
    if (x)
        printf("x is non null\n");
    else
        printf("x is 0\n");
    for (int i = 0; i<10; i = i +1)
        printf("i is %d\n", i);
    ```
    

Warning: if adding a second statement, put braces back

Also ambiguity:

```c
if(one)
    if(two)
        foo();
  else           // whose else is this?
        bar();
```

## Functions (Video 3)

Functions have a name, zero or more parameters with types, and a return type

Like variables functions must be declared before being used

```c
#include <stdio.h>

int add_two_integers(int a, int b) {
    int result = a + b;
    return result;
}
int main() {
    int x = 1;
    int y = 2;
    int sum = add_two_integers(x, y);
    printf("result: %d", sum);
    if(add_two_integers(x, y))
        printf(" (non zero)\n");
    else
        printf(" (zero)\n");
    return 0;
}

>>> 
result: 3 (non-zero)
```

The absence of return value can be indicated with the `void` type

```c
void print_hello(void) {
    printf("hello!\n");
    return; // we can even omit this as the function does not return anything
}
int main() {
    print_hello();
    return 0;
}
```

### Function Calls

Function parameters and return values are passed as copies, not by reference

```c
void my_function(int parameter) {
    parameter = 12; // does not update x in main
}
int main() {
    int x = 10;
    my_function(x);
    printf("x is %d\n", x); // prints 10
    return 0;
}
>>> x is 10

/*
As seen, value for x is modified in my_function function as value is overwritten
however, in C, return values are passed as copies which is why it retains its 
value and doesn't change to 12 but 10
*/
// This is however done through pointers
```

### Forward Declarations

The compiler parses source files from top to bottom

- One can declare a function before 1) using it ; and 2) during defining it

```c
#include <stdio.h>
/* Forward declaration, also called function _prototype_ */
int add(int a, int b);
int main(int argc, char **argv) {
    int a = 1;
    int b = 2;
    /* Here we need the function to be at least declared -- not necessarily defined */
    printf("%d + %d = %d\n", a, b, add(a, b));
    return 0;
}
/* The actual function definition */
int add(int a, int b) {
    return a + b;
}

>>> 1 + 2 = 3
```

### Variables Scope and Lifetime

Global variables are declared outside functions

- Visible at the level of the entire source file

```c
int global_var;
void add_to_global_var(int value) {
    global_var = global_var + value;
}
int main() {
    global_var = 100;
    add_to_global_var(50);
    printf("global_var is %d\n", global_var);
    return 0;
}
>>> global_var is 150
```

Try to limit the use of globals as much as possible

- A lot of global state makes it harder to reason about the program

Local Variables

- Within a function, a declared local variable is accessible within it’s enclosing braces (a block of code)
    
    ```c
    #include <stdio.h>
    
    int main(){
        int x = 12;
        if(x) {
            int y = 14;
            printf("inner block, x: %d\n", x);
            printf("inner block, y: %d\n", y);
        }
        printf("outer block, x: %d\n", x);
        return 0;
    }
    >>> 
    inner block, x: 12
    inner block, y: 14
    outer block, x: 12
    
    #include <stdio.h>
    
    int main(){
        for(int i=0; i<10; i++) {
            printf("In loop body, i is %d\n", i);
        }
        // ERROR -- i only visible in loop body
        // printf("Out of loop body, i is %d\n", i);
        int j;
        for(j=0; j<10; j++) {
            printf("In loop body, j is %d\n", j);
        }
        // WORKING -- j declared in current block
        printf("Out of loop body, j is %d\n", j);
        //
        return 0;
    }
    >>>
    In loop body, i is 0
    In loop body, i is 1
    In loop body, i is 2
    In loop body, i is 3
    In loop body, i is 4
    In loop body, i is 5
    In loop body, i is 6
    In loop body, i is 7
    In loop body, i is 8
    In loop body, i is 9
    In loop body, j is 0
    In loop body, j is 1
    In loop body, j is 2
    In loop body, j is 3
    In loop body, j is 4
    In loop body, j is 5
    In loop body, j is 6
    In loop body, j is 7
    In loop body, j is 8
    In loop body, j is 9
    Out of loop body, j is 10
    ```
    
    It is good practice for clarity to declare local variables at the beginning of their block
    

## Custom Types and Data Structures (Video 4)

### Custom Types

We can use `typedef` to alias a type

```c
#include <stdio.h>
// 'my_int' is now equivalent to 'long long unsigned int'
typedef long long unsigned int my_int;
int main(int argc, char **argv) {
    my_int x = 12;
    printf("x is: %llu\n", x);
    return 0;
}
>>> x is: 12
```

### Custom Data Structures

- Use `struct` to define a data structure with named fields
- Use `struct <struct name>` to refer to it as a type
- Access the fields of a variable with `<variable_name>.<field_name>`

```c
#include <stdio.h>
#include <string.h> // needed for strcpy

struct person {
    char name[10];
    float size_in_meters;
    int weight_in_grams;
};

void print_person(struct person p) {
    printf("%s has a size of %f meters and weights %d grams\n",
            p.name, p.size_in_meters, p.weight_in_grams);
}

int main(int argc, char **argv) {
    struct person p1;

    strcpy(p1.name, "Julie");
    p1.size_in_meters = 1.6;
    p1.weight_in_grams = 60000;

    struct person p2 = {"George", 1.8, 70000};

    print_person(p1);
    print_person(p2);
    return 0;
}
>>> 
Julie has a size of 1.600000 meters and weights 60000 grams
George has a size of 1.800000 meters and weights 70000 grams
```

- Fields are placed consecutively in memory (compiler may insert padding)
    
    ![Untitled](Week%202%2085f534485d3c41ceb23bea8f9842d0a5/Untitled%208.png)
    

Structures can be `typedef’d`

```c
#include <stdio.h>
#include <string.h> // needed for strcpy
struct s_person {
    /* fields declaration ... */
};
typedef struct s_person person;
void print_person(person p) { /* ... */}
int main(int argc, char **argv) {
    person p1;
    person p2 = {"George", 1.8, 70000};
    /* ... */
}
```

A faster way to do this is

```c
typedef struct {
    /* fields declaration ... */
} person;
```

### Enumeration

User defined type for assigning textual names to integer constants

Constants are often in capital letters in C

```c
enum color {
    RED,
    BLUE,
    GREEN
};
int main(int argc, char **argv) {
    enum color c1 = BLUE;
    switch(c1) {
        case RED:
            printf("c1 is red\n"); break;
        case BLUE:
            printf("c1 is blue\n"); break;
        case GREEN:
            printf("c1 is green\n"); break;
        default:
            printf("Unknown color\n");
    }
    return 0;
}
>>> c1 is blue
```

Under the hood the compiler assigns an integer constant to each value of an `enum`

```c
printf("BLUE was assigned: %d\n", BLUE);
printf("GREEN was assigned: %d\n", GREEN);
printf("RED was assigned: %d\n", RED);
```

Enumerations can be `typedef`

```c
enum {
    BLUE,
    /* ,,, */
} e_color;
enum e_color c1 = BLUE; // without typedef
typedef enum e_color color;
color c2 = RED;
```

All in once:

```c
typedef enum {
    BLUE,
    /* ... */
} color;
```