[[COMP26020]]

### Compilation
#### Header Inclusion
**Header files:** `C` code file with `.h` extension, contains what is necessary to use foreign libraries/source files
- Should be included in the source (`.c`) file wishing to use the foreign code
```c
// In the .c source file, include headers like that:
#include <stdio.h>            // Use <> to include a file from the default includepath
#include "my-custom-header.h" // Use "" for the local and user-supplied include directories
```
- Headers can be included in several `.c` source files composing a program
- To avoid double definitions, they should only contain declarations: functions prototypes, variables/custom types/structs declaration, e.t.c.
- No definitions (i.e. implementation)

#### Macro Expansions
- Textual substitutions, useful for compile-time defined constants
	- Generally indicated in capital letters as a convention
![[Pasted image 20230421234227.png]]
In general, try to have less hardcoded values as possible
- When you observe that you are writing a constant value more than once, check if using a macro is possible
- Even if you have a constant used just once, a macro can make it look more meaningful and easy to update in the future
Macros can also be used to write macro "functions"
> Be careful with operator precedence and use brackets:
![[Pasted image 20230421234515.png]]
#### Conditional Compilation
```c
#define DEBUG_MODE // controls the activation / deactivation of debug mode
#define VERBOSITY_LEVEL 4 // control the debug verbosity level

// comment the first line to disable debug mode

#if VERBOSITY_LEVEL > 3 // You can use the other C-like comparison operations (==, etc.)    printf("Additional debug because the verbosity level is high\n");#endif /* VERBOSITY_LEVEL */
```
![[Pasted image 20230421234820.png|550]]

-----------------------------------------------------------------------
#### Modular Compilation

Large programs can consist of up to 10,000s of files so how can we even begin to compile this? Are we seriously going to `gcc <10000 sources files>` each time we do a small update in a couple files? Can we automate this in some way?

![[Pasted image 20230327171047.png|600]]

Let's break this down into a problem with multiple modules:

![[Pasted image 20230327171148.png|500]]

```bash
gcc -c main.c -o main.o
gcc -c parser.c -o parser.o
gcc -c network.c -o network.o
gcc main.o network.o parser.o -o prog
```

When we want to compile after changes we have to know which files are dependent on which...

![[Pasted image 20230424103944.png]]
![[Pasted image 20230327172154.png|600]]

 - There is generally 1 header file per module, defining it's interface.
 - Note that this `.h` file can be included in many `.c` files and thus it can only contain ==declarations==, no definitions (function bodies and global variables)

So this is the structure:

![[Pasted image 20230327180215.png|300]]

So we can use makefiles to help us with this:

![[Pasted image 20230327182322.png]]

#### Automated Compilation
```make
# The first rule is executed when the
# command make is typed in the local folder:
all: prog

# executable deps and command to buil1d it:
prog: main.o network.o parser.o
gcc main.o network.o parser.o -o prog

# network.o deps and command to build it:
network.o: network.c network.h
gcc -c network.c -o network.o

parser.o: parser.c parser.h network.h
gcc -c parser.c -o parser.o

main.o: main.c network.h parser.h
gcc -c main.c -o main.o

# Special rule to remove all build files
clean:
rm -rf *.o prog
```
C/C++ application build automation and dependency management with Makefiles

#### Type Conversions
Often the case is that the smaller values when doing operations is promoted to the larger value...
`long long result = (char + int) + ll` - here char is promoted to integer and then integer is promoted to long long..

Let's look at this in more detail:
Integer Promotion

![[Pasted image 20230327194554.png|300]]

If we have 2 variables of...
1. Same type - no promotion
2. Same signed/unsigned attribute? Lesser rank promoted to greater rank
3. Unsigned rank >= other operand rank? Signed promoted to the unsigned rank
4. Signed type can represent all values of the unsigned? Unsigned operand promoted to signed type.
5. Otherwise both operands converted to unsigned type corresponding to the signed operands type.

```c
int main(int argc, char **argv) {

//prints 7: 25/10 rounds to 2; 2 * 15 = 30; 30/4 rounds to 7
printf("%d\n", 25 / 10 * 15 / 4);

// prints 7.5: 25/10 rounds to 2; 2*15 = 30; 30 gets converted to
// 30.0 (double) and divided by 4.0 (double) giving result 7.5 (double)
printf("%lf\n", 25 / 10 * 15 / 4.0);

// prints 9.375: 25.0 / 10.0 (converted from 10) is 2.5, multiplied by 15.0
// (converted from 15) gives 37.5, divided by 4.0 (converted from 4) gives
// 9.375
printf("%lf\n", 25.0 / 10 * 15 / 4);

// prints garbage, don't try to interpret a double as an int!
printf("%d\n", 25.0 / 10 * 15 / 4);

return 0;

}
```

Casting:
`(type)expression`

```c
// prints 3.75: 4 gets converted to 4.0
printf("%lf\n", (float)15/4);

// prints 4: 2.5 converted to 2 (int), multiplied by 12 gives 24, divided
// by 5 gives 4
printf("%d\n", ((int)2.5 * 12)/5);

// prints 4.8: 2*12 = 24, converted to 24.0, divided by 5.0 gives 4.8
printf("%lf\n", ((int)2.5 * 12)/(double)5);

return 0;
```

You can also cast with pointers as well! `(char *) ptr`

C and C++ are extensively used in HPC because of their speed
- Run close to the hardware
- No additional software layers
- no runtime overhead
- integrates well with assembly
- control of the memory layout

### Generic Pointers
Generic pointers are pointers of the same type that can point to data of differenet types.
![[Pasted image 20230509205536.png|550]]
Output:
![[Pasted image 20230509205619.png|550]]

### Some Pointers for the Exam

Think of Macros as literally textual substitution!
```cpp
#define ten                       (10 + 1)  

int a = ten + ten;
```
Is exactly the same as:
```cpp
int a = (10 + 1) + (10 + 1); 
```

--------

By convention, up to 6 arguments for system calls, for x86 are passed in order in `%rdi`, `%rsi`, `%rdx`, `%r10`, `%r8`, and `%r9`. The rest of the arguments will be passed via the stack.

And the return value of the function is returned via `%rax` register.

-----------

