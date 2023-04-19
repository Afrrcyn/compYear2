[[COMP26020]]

### Pointers
- Would you like a function to change it's parameters - this seems impossible when arguments are passed by copy in C..
- Want a function to return more than a single value?
- What if we're passing a massive data structure, we're going to have to copy all of it's content to another twin variable, this is not efficient.. - doing this causes a huge performance drop and massive memory footprint increase...

With a pointer, we can solve these problems...

```c
typedef struct { double a; double b; double c; double d; double e; double f;} large_struct;

void f(large_struct *s) { // now takes a pointer parameter
(*s).a += 42.0; // dereference to access x
return;
}

int main(int argc, char **argv) {
large_struct x = {1, 2, 3, 4, 5, 6};
f(&x); // pass x's address
printf("x.a: %f\n", x.a);
return 0;
}
```

A pointer allows us to simply pass an address which can be a 8 byte number instead of a copying a massive data structure and this means we can directly access the original variable so no copying and we can the original variable, now you can see how this solves our original 3 problems.

Now that you sorta know what pointers are, they simply point to the address of a variable. We can let you know that C arrays are also just pointers...

```c
int int_array[2] = {1, 2};
double double_array[2] = {4.2, 2.4};
char char_array[] = "ab";

printf("int_array[0] = %d\n", int_array[0]);
printf("int_array[1] = %d\n", int_array[1]);
printf("*(int_array+0) = %d\n", *(int_array+0)); // pointer arithmetic!
printf("*(int_array+1) = %d\n", *(int_array+1)); // +1 means + sizeof(array type)
printf("double_array[0] = %f\n", double_array[0]);
printf("double_array[1] = %f\n", double_array[1]);
printf("*(double_array+0) = %f\n", *(double_array+0));
printf("*(double_array+1) = %f\n", *(double_array+1));
printf("char_array[0] = %c\n", char_array[0]);
printf("char_array[1] = %c\n", char_array[1]);
printf("*(char_array+0) = %c\n", *(char_array+0));
printf("*(char_array+1) = %c\n", *(char_array+1));
```

![[Pasted image 20230327141703.png|600]]

You can declare a pointer like such: `<type> *name = &variable`, here `&` will return the address of a variable. and say if `name` was a structure we can access it's field variables in 2 ways: `name->field_var` or `(*name).field_var`

A pointer itself is a variable with an address so you can have a pointer pointing to it...

```c
int val = 42;
int *ptr1 = &val;
int **ptr2 = &ptr1;
int ***ptr3 = &ptr2;
```
where the type of `**ptr2` is a pointer to a pointer of int.

### Malloc
The stack is a small size, mostly allocated at compile time.
The heap is large and all allocations are made dynamically at runtime.

So we can actually allocate on the heap in C, firstly we need to include a library `#include <stdlib.h>`, and from there we can do:

```c
heap_array = malloc(size * sizeof(int))
if (heap_array == NULL) return -1;
...
free(heap_array)
```

`malloc()` allocates a chunk of memory on the heap and returns a pointer to it. It will return `NULL (0)` if the allocation fails, so always check the return type... And once you're done, don't forget to free the variable!

For a multi-dimensional array, you'll have to malloc the first dimension and then for each index in the first dimension, malloc the second dimension for that first...

```c
array = malloc(a * sizeof(int *));
if(array == NULL) return -1;
for(int i=0; i<a; i++) {
array[i] = malloc(b * sizeof(int));
if(array[i] == NULL) return -1;
}
```

But the key thing here to remember is that the first dimension is not a standard variable type but rather a pointer! Do not forget this!

### Stdlib

##### String
We have to standard copy functions 
```c
char *strcpy(char *dest, const char *src);
char *strncpy(char *dest, const char *src, size_t n);
```
The second one being better because what happens if the string you're copying to is smaller than the string you're copying? So use `strncpy()` where you can!

We also have functions for concatenation
```c
char *strcat(char *dest, const char *src);
char *strncat(char *dest, const char *src, size_t n);
```
With the second one giving more control allowing it to be safer!

```c
int sprintf(char *str, const char *format, ...);
```
creates a string, very similar idea to that of `printf()`
```c
char string[64];
sprintf(string, "a is %d, b is %f, s is %s\n", a, b, s);
printf("%s", string);
```

Some more:
```c
size_t strlen(const char *s);
int strcmp(const char *s1, const char *s2);
```

where `strcmp` may return `0`, if the strings are equal.

##### Console Input

```c
char *fgets(char *s, int size, FILE *stream);
int scanf(const char *format, ...);
```
`fgets` gets a string of a certain size, whereas `scanf` can input several different types of variables.

##### Memory
We have two memory functions:
```c
void *memset(void *s, int c, size_t n);
void *memcpy(void *dest, void *src, size_t n);
```

for these functions we need to include `#include <string.h>`, so here `memset()` will set all memory locations to a certain value. Obviously please remember to allocate the memory you are setting.

##### Math
To include simply do `#include <math.h>`, we can use functions like:
```c
double ceil(double x);
float ceilf(float x);
double sqrt(double x);
double pow(double x);
double cos(double x);
```
These do exactly what you think and you can find more at this link: [here x](http://www.cplusplus.com/reference/cmath/)

##### Sleeping and Time

```c
unsigned int sleep(unsigned int seconds);
int usleep(useconds_t usec);

// usage:
sleep(100);
```

and to use these functions we need `#include <unistd.h>`

```c
time_t time(time_t *tloc); // time_t is generally a long long unsigned int (llu)

// usage:
long long int a = time(NULL)
// this gives us the time elapsed since the epoch (01/01/1970)
```

There's also a function to determine the time elapsed since epoch and helps to measure execution time:

```c
int gettimeofday(struct timeval *tv, struct timezone *tz);

struct timeval {
time_t tv_sec; /* seconds (type: generally long unsigned) */
suseconds_t tv_usec; /* microseconds (type: generally long unsigned) */
}; // pretty sure we don't have to define this!

struct timeval start, stop, elapsed;
gettimeofday(&start, NULL);
for(int i=0; i<1000000000; i++);
gettimeofday(&stop, NULL);
timersub(&stop, &start, &elapsed);

printf("Busy loop took %lu.%06lu seconds\n", elapsed.tv_sec, elapsed.tv_usec);
```

Where we include `#include <sys/time.h>` to use the function...

##### File
```c
int open(const char *pathname, int flags, mode_t mode);
```

`open` creates a file descriptor, we also have different modes and flags and can combine them using `|`

```c
int file_descriptor = open("/home/folder/test", O_RDWR | O_CREAT, S_IRWXU);
```

We can also read:
```c
ssize_t read(int fd, void *buf, size_t count);
```
attempts to read a specified number of bytes from a file, where `buf` contains the buffer where the characters are read into, the function will return the number of bytes read.

```c
int bytes_read = read(file_descriptor, buffer, 10);
if(bytes_read == -1) {
	/* error */
} else if(bytes_read != 10) {
	/* technically not an error */
	// might have reached EOF
}
```

For writing we have a very similar interface that works backwards
```c
ssize_t write(int fd, const void *buf, size_t count);
```
We will attempt to write `count` bytes from a buffer `buf` to a file, and we return the number of bytes written...

Now once done we must also close a file!
```c
int close(int fd);
```

##### Random

There's a function `rand()` that returns a random number from `0` to `RAND_MAX`, and if we want maybe a number from `0` to `100` we could do `rand() % 100`, please don't forget if you can set the seed at the start of running the program to ensure it's always random: `srand(time(NULL));`

##### Errors
```c
#include <errno.h> // needed for errno and perror

int main(int argc, char **argv) {
	int fd = open("a/file/that/does/not/exist", O_RDONLY, 0x0);

/* Open always returns -1 on failure, but it can be due to many different reasons */

	if(fd == -1) {
		printf("open failed! errno is: %d\n", errno);
		/* errno is an integer code corresponding to a given reason. To format
		* it in a textual way use perror: */
		perror("open");
}
return 0;
}
```

##### Cmd Line Input

So remember earlier on.. [[PLP/C/Week 1/Introduction#^f3158f|atoi]] - I said this.. well there are actually some problems with it, how do we detect malformed strings? `atoi` does not offer any way to know the string in invalid or overflows or underflows an `int`...

The solution? Use:

```c
long strtol(const char *nptr, char **endptr, int base);
```

- Coverts the string `nptr` into a long integer and return that integer
- `base` can be 10 (decimal), 8 (octal) or 16 (hex)
- After the call `*endptr` will either point to `\0` - if the string was valid or something else if the string was not valid.
- Any under/overflows cause `errno` to be set to `ERANGE` and the function returns `LONG_MIN` (for an underflow) or `LONG_MAX` (for an overflow)

A common usage:

```c
#include <errno.h>
#include <limits.h>

int main(int argc, char **argv) {
if(argc != 2) { /* handle incorrect execute usage */ }
char *endptr;
long n = strtol(argv[1], &endptr, 10);
if(*endptr != '\0') {
	printf("invalid string!\n");
	return -1;
}

if(errno == ERANGE) {
	if(n == LONG_MIN) printf("underflow!\n");
	if(n == LONG_MAX) printf("overflow!\n");
	return -1;
}

printf("n is: %ld\n", n);
return 0;
}
```

##### Stream-based File I/O

```c
FILE *fopen(const char *pathname, const char *mode);
```

- This opens the file determined by the `pathname` and returns a stream object `FILE * stream` or returns `NULL` on error..
- There are several types of `modes`, those being: 
	- `r` - read only
	- `r+` - read and write
	-  `w` -  write only, truncate file if it exists, create if not
	- `w+` - read and write, trunc if it exists and create if not

Use 
```c
int fclose(FILE *stream);
```
to close the file.

Reading and Writing:
```c
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream);
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream);
```

This is slightly confusing as there's now `size_t size` and `size_t nmemb`, the difference `size` refers to the size of each object we are reading, for example a `char` would be 1 byte, an `int` may be 4 bytes, and `nmemb` refers to how many objects we'll be reading...

```c
items_read = fread(buffer, 4, 3, file_ptr);
```
Is 3 items of 4 bytes - so in total 12 byes, so we need to ensure out `buffer` can handle this..

An example:

```c
#include <stdio.h>
char *alphabet = "abcdefghijklmnopqrstuvwxyz";

int main(int argc, char **argv) {

FILE *f1, *f2;
char buffer[27];
f1 = fopen("test-file.txt", "w");

if(f1 == NULL) {
	perror("fopen");
	return -1;
}

if(fwrite(alphabet, 2, 13, f1) != 13) {
	perror("fwrite");
	fclose(f1);
	return -1;
}
fclose(f1);

f2 = fopen("test-file.txt", "r");

if(f2 == NULL) {
	perror("fopen");
	return -1;
}

if(fread(buffer, 1, 26, f2) != 26) {
	perror("fread");
	fclose(f2);
	return -1;
}

buffer[26] = '\0';
printf("read: %s\n", buffer);
fclose(f2);
return 0;
}
```



### Key things to know

If you have a struct, say:
```cpp
struct {
	int age;
	int weight;
	int height;
} Human;
```
And say we have:
```cpp
Human h = ...
```
The address `&h`, is actually the address of the first attribute `age` and so `weight` is `&h + 0x4` and `height` -> `&h + 0x8`

-----------------------------------

Also try to have an understanding of what the return values of functions mean, like `strcmp`, will actually return `0`, if the strings are the same, so doing:
```cpp
if (strcmp(s1, s2)) ...
```
This only takes the if branch, if the strings are not equal...

------------

You can find global variables by any variable declared outside all blocks:
```cpp
int hi;

int main(int argc, char **argv) {...}
```
And all global variables are stored in static memory, all local variables that are not defined via `malloc` are stored in the stack and anything defined by `malloc` is stored in the heap. And *i think*, all pointers are stored on the stack.