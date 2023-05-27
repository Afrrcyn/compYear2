[[COMP26020]]

C is the default language to write OS kernels
So let's look at the implementation of
- System calls handling
- Context Switch (replacement of a task running on a CPU by another one)

Let's look at a ==system call==
- The CPU is busy with a task in user code and encounters a System Call, so it switches to the supervisor mode, where it will use a Kernel Stack and run Kernel mode, it pushes registers to save the user code's registers and performs the required tasks, before switching back to user mode, it pops all registers so the user mode can continue as it were.

```arm
; User Code
ADD R2, R4, R3 LSL #2
SVC 2 ; System Call
; return here
...

; Kernel Code 
; SVC 2 takes us here
PUSH {R0, R1, ...}
... ; perform task
POP {R0, R1, ...}
MOVS PC, LR ; return to user mode
```

Now let's look at a ==context switch==.

This is not necessarily doing system calls but rather say the CPU has 3 tasks to do, every now and then it will have to switch tasks from task A to task B, each task having their own register values, so switching requires flushing the CPU with PUSHs and reloading it with POPs from another Tasks stack.

Often this switch can be done via an interrupt which say interrupts every 10ms to allow another task to do it's thing.

### Context Switching and System Call in HermiTux
- Context switching and system call handling can be done only by the kernel (supervisor mode?)
- Only way to enter in kernel mode is through an interrupt
- Interrupt traps to the kernel which saves the state of the interrupted task for resuming properly later

### Memory Safety
C and C++ are inherently memory unsafe.
There is absolutely no check at runtime if memory accessed is initialised, allocated, or mapped
Compiler does not warn you at times as well
-   **A few examples of memory errors:**
    -   Out of band accesses on buffers/arrays accesses
    -   Failure to initialise before read stack/heap allocated variables
    -   Leaks, double free, use-after-free
Memory errors are **hard to detect, hard to debug**
-   No warning/error at compile-time
-   Undefined behaviour: at runtime sometimes it crashes, sometimes not...
##### Example: Infoleak
![[Pasted image 20230510001613.png]]
From this screenshot, we can see that if we reduced the welcome message, we forgot to update the for loop, which causes overflows and leaks

##### Example: Sensitive Data Tampering
Sensitive data tampering in C with regard to memory safety refers to the manipulation of data that is considered sensitive, such as passwords, financial information, or personal identifiable information, by exploiting memory safety vulnerabilities in C code.

Memory safety vulnerabilities can arise from a variety of sources, such as buffer overflows, use-after-free errors, and format string vulnerabilities. These vulnerabilities can allow an attacker to modify data in memory that they should not have access to, including sensitive data.

For example, if a C program is vulnerable to a buffer overflow attack, an attacker can provide input that overflows the buffer and overwrites adjacent memory, including sensitive data. Similarly, a use-after-free vulnerability can allow an attacker to modify data in memory that has already been freed, including sensitive data.

##### Example: Stacking Smashing
Stack smashing in C is a type of memory safety vulnerability that occurs when a program writes beyond the allocated size of a buffer on the stack. This can cause data corruption and lead to security vulnerabilities, such as arbitrary code execution, denial-of-service attacks, or data leakage.

The stack is a region of memory that is used by C programs to store local variables, function parameters, and return addresses. When a function is called, a new stack frame is created on top of the current stack, which contains the function's local variables and other data. If a buffer on the stack is not properly sized or bounded, then an attacker can exploit this vulnerability by overwriting adjacent memory, including the return address, which can cause the program to execute arbitrary code.

Stack smashing attacks typically involve providing input to a program that overflows a buffer on the stack, causing the program to crash or execute unexpected behavior. Attackers can use various techniques, such as crafted input data or carefully crafted function call chains, to trigger stack smashing vulnerabilities in C programs.

##### Example: Use after free
Use-after-free in C is a type of memory safety vulnerability that occurs when a program continues to use a memory address after it has been freed. This can cause undefined behavior, such as crashes, data corruption, or arbitrary code execution, and can be exploited by attackers to gain control of a program.

In C, memory allocation and deallocation are typically managed using functions such as malloc(), calloc(), and free(). When memory is allocated using these functions, the program obtains a pointer to the allocated memory, which can be used to read and write data. When the memory is no longer needed, the program frees the memory using the free() function, which returns the memory to the system.

However, if a program continues to use a memory address after it has been freed, it can cause undefined behavior. For example, the memory might be overwritten with new data, or the program might crash when it tries to access the freed memory. This can be exploited by attackers to modify data or execute arbitrary code, by manipulating the freed memory to point to attacker-controlled data or code.

### Good Practices
#### Tools
Enable extra warnings with compiler flags:
- `-Wall`: Enables first set of warning
- `-Wextra`: Enables extra sets of warnings
- `-pedantic`: Write C code that conforms strictly to ISO C Standard

##### Dynamic Analysis
- Valgrind: Focuses on heap and is very practical to check for memory leaks
- Address Sanitizer (ASAN): Can detect overflows, use after frees, leaks, ...
	- Also when ASAN detects a bad memory access, you get a crash log of exactly where what went wrong and at the end of execution you get a summary of possible leaks
- BoundCheck, Purify, e.t.c.

##### Static Analysis (without running program code)
- Clang Static Analyser: Checks for division by zero, null pointer references, usage of uninitialised values, ...
- Lint, Coverity, cppcheck, e.t.c.

### Undefined Behaviour
![[Pasted image 20230509230300.png|500]]

### Array / Buffers Sizes, Integer Overflows
- Keep track of arrays sizes
- Be aware of type sizes to avoid overflows
	- `sizeof()`
	- Signed overflow: undefined behaviour

### C Standard Library
Never use these functions because they are unsafe:
- gets(use fgets)
- getfwd(use getcwd)
- readdir_r(use readdir)

### String Manipulation Functions
- Use `n` methods
	- `strcpy` -> `strncpy`
	- `sprintf` -> `snprintf`
- Even with versions with `n` some particularities:
	- `strncpy` won't add `\0` at the end of the target buffer
![[Pasted image 20230509230826.png]]

### Dynamic Memory Allocation
- Check `malloc` return value
- After free, pointer becomes invalid
	- Cannot be dereferenced
	- Cannot be used (ex: comparison)
- `realloc` returns null upon failure but does not free the old pointer
	- `ptr = realloc(ptr, newsize)` is a leak

### For the Exam

Context Switching and System Calls are both written in a mixture of `C` and `Assemlbly`, both of which are memory unsafe...
Assembly is necessary for low level operations such as saving CPU state or switching tasks

Use-after-free bugs are generally not detected by the compiler, they put the program into undefined behaviour and can allow an attacker to subvert its control flow. They do not necessarily lead to a crash at runtime.