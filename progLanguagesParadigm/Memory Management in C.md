[[COMP26020]]

## Introduction to Pointers (Slide 1)

### Program Memory Layout

- All the program’s code and data is present somewhere in memory
- The area of memory accessible by the program is the address space
    - It is a large array of contiguous bytes
    - **Addresses** are indexes in that array
    
    ![Untitled](Week%203%2019f00b77e1d544ee9d09c746939048eb/Untitled.png)
    

- Note that this is virtual memory:
    - Address space size independent from the amount of RAM
    - Each program gets it’s own address space
- Address of a variable:
    
    The address in memory of the first byte storing that variable
    
    ```c
    int glob = 12;             // glob's address is 0x3300
    char string[] = "abcd";    // string's address is 0x2f0
    typedef struct {
        int member1;
        float member2;
    } mystruct;
    int main(int argc, char **argv) {
        mystruct ms;            // ms' address is 0x123
        ms.member1 = 42;
        ms.member2 = 4.2;
        printf("ms member1: %d, member2: %f\n", ms.member1, ms.member2);
        return 0;
    }
    ```
    
    ![Untitled](Week%203%2019f00b77e1d544ee9d09c746939048eb/Untitled%201.png)
    

### Addresses

Use the `&` operator to get the address of a variable

```c
int glob = 12;
typedef struct {
    int member1;
    float member2;
} mystruct;
int main(int argc, char **argv) {
    mystruct ms = {1, 2.0};
    // With modern processors an address is a 64 bits value so we need the
    // right format specifier: `%lu` or `%lx` if we want to print in hexadecimal
    printf("ms address is: 0x%lx, glob address is 0x%lx\n", &ms, &glob);
    return 0;
}
>>> ms address is: 0x7ffcba5cd750, glob address is 0x56392c797010
```

### Pointers

Variable that contains an address (possibly of another variable)

Declaration: `<pointed type> *<pointed name>;`

```c
int v = 42;
int *ptr = &v;

printf("%d, %p\n",v, ptr);
>>> 42, 0x7ffcff81487c
```

![ptr holds the address of variable v](Week%203%2019f00b77e1d544ee9d09c746939048eb/Untitled%202.png)

ptr holds the address of variable v

**Dereferencing** (accessing the pointed value)

Used by `*` operator

```c
printf("pointer value:   0x%lx\n", ptr);  // 0xF123, can also use %p (just use %p)
printf("pointed value:   %d\n", *ptr); //42
```

### Pointers - Example

```c
typedef struct {
    int member1;
    double member2;
} mystruct;

int main(int argc, char **argv) {
    mystruct ms = {55, 2.23};

    int *ptr1 = &glob;
    double *ptr2 = &glob2;
    mystruct *ptr3 = &ms;

    printf("ptr1 = %p, *ptr1 = %d\n", ptr1, *ptr1);
    printf("ptr2 = %p, *ptr2 = %f\n", ptr2, *ptr2);
    printf("ptr3 = %p, *(ptr3).member1 = %d, *(ptr3).member2 = %f\n",
            ptr3, (*ptr3).member1, (*ptr3).member2);
    return 0;
}
>>> 
ptr1 = 0x5640bb7d7010, *ptr1 = 12
ptr2 = 0x5640bb7d7018, *ptr2 = 4.400000
ptr3 = 0x7ffc02204b10, *(ptr3).member1 = 55, *(ptr3).member2 = 2.230000
```

![Untitled](Week%203%2019f00b77e1d544ee9d09c746939048eb/Untitled%203.png)

### Pointer:

- Variable that stores an address corresponding to a memory location
- Can access that location through the pointer

## Pointers Applications (Slide 2)

To change a parameter of a function

Arguments are passed by copy in C

```c
void add_one(int param) {
    param++;
}
int main(int argc, char **argv) {
    int x = 20;
    printf("before call, x is %d\n", x);   // print 20
    add_one(x);
    printf("after call, x is %d\n", x);    // print 20
    return 0;
}
// no change in value of x even after param was incremented
```

![Untitled](Week%203%2019f00b77e1d544ee9d09c746939048eb/Untitled%204.png)

Use a pointer argument to pass the address as parameter

```c
void add_one(int *param) {
    (*param)++;
}
int main(int argc, char **argv) {
    int x = 20;
    printf("before call, x is %d\n", x);   // print 20
    add_one(&x);
    printf("after call, x is %d\n", x);    // print 21
    return 0;
}
// change in value of x after pointer argument has been passed
```

![Untitled](Week%203%2019f00b77e1d544ee9d09c746939048eb/Untitled%205.png)

Return more than a single value

```c
// we want this function to return 3 things: the product and quotient of n1 by n2,
// as well as an error code in case division is impossible
int multiply_and_divide(int n1, int n2, int *product, int *quotient) {
    if(n2 == 0) return -1;      // Can't divide if n2 is 0
    *product = n1 * n2;
    *quotient = n1 / n2;
    return 0;
}
int main(int argc, char **argv) {
    int p, q, a = 5, b = 10;
    if(multiply_and_divide(a, b, &p, &q) == 0) { // == 0 is the return value in mult_and_divide
        printf("10*5 = %d\n", p); printf("10/5 = %d\n", q);
    }
}
>>>
10*5 = 50
10/5 = 0
```