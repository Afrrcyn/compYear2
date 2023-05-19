[[COMP26020]]

C++ started out at C with classes but now it's much more than just a C extension

80s: Pre-standard C++ 
- C with Classes

90s/00s: C++98  
-  Templates + Template Library

2011+: C++11  
- Functional
- Compile-time evaluation
- Simpler syntax
- Better type safety
- Threading
- ...

Why should we use C++?
- It's pragmatic, 
- Very fast - only has zero-cost abstractions and High Level
- RAII - Resource allocation is automated through the compiler - this allows certain operations which may be complicated, to be done easily with some sense of abstraction.
- Low overhead
- Large Codebase and community
- Highly popular

But it's not perfect
- Complex
- Dangerous
- No garbage collection
- And designed by a committee..

Some of the immediate differences we can look at are:

##### Printing
```cpp
#include <iostream>

std::cout << "Hello World!\n";
```

Here you can see the concatenation operator is `<<` and we had to include a cpp library to run this.

##### Dynamic Memory Allocation

In C this would look something like this:
```c
int *heap_num = malloc(sizeof(int));
*heap_num = 3;
free(heap_num);
```

In Cpp:
```cpp
int *heap_num = new int;
*heap_num = 3;
delete heap_num;

// or

int *heap_num = new int(3);
delete heap_num;
```

Well, what about arrays?

In C:

```c
int *heap_arr = malloc(sizeof(int) * size);
...
free(heap_arr);
```

And in Cpp:
```cpp
int *heap_arr = new int[size];
...
delete heap_arr;
```

##### Yeah so
***Don't*** use `malloc` it's considered harmful
and here's the part where I confuse you
***Don't*** use `new`, it's also considered harmful...

##### Creating Variables
We now have a new type in Cpp, `bool` which has values `true` or `false`

Struct variables declared without the struct keyword now.

```cpp
struct S { int first; int second};
S x;
```

There are also more ways to initialise a variable now.

```cpp
int num = 5; //ok-ish, C-like casting rules
int num = 5.5; //ok? 5.5 implicitly cast to (int)(5)

int num(5); //ok, but preferred for function calls

int num{5}; //ok, preferred for direct initialisation
int num{5.5}; //Error, 5.5 is not an int
int nums[]{1, 2, 3, 4, 5}; //nums[0] = 1, nums[1] = 2...
S x{0, 1}; //x.first = 0, x.second = 1;
```

##### References
Cpp has a new way to access variables - along with normal variables and pointers, we have references. References point to the memory address of the original variable such that any changes to the new variable will change the original variable and we use the keyword `&` to show something is a reference.

```cpp
int size = 4;
int& size1 = size;
// size = 4, size1 = 4

size1 += 3;
// size1 = 7, size = 7
```

So isn't the reference achieving the same thing as a pointer? Well a reference is slightly different in that it allows the syntax to remain the same as a normal variable without the slightly confusing pointer syntax.. A reference must always be ==initialised== and can never be ==null== and finally it must point to the same data for it's entire lifetime.

##### Function Overloading
This essentially allows for a function to have the same name but different parameters and the program will decide which one to call based on the parameters supplied when called.

```cpp
void print(int x) {
	std::cout << "Integer: " << x << "\n";
}

void print(char x) {
	std::count << "Character: " << x << "\n";
}

int y = 1;
print(y);
// prints: Integer: 1
```

##### Classes
Classes in Cpp aren't that different from Java

```java
class Circle {
	double x, y, r;
	
	...
	
	double getArea() {
		return 3.14 * this.r * this.r;
	}
}
```

and in Cpp:
```cpp
class Circle {
	double x, y, r;
	
	...
	
	double getArea() {
		return 3.14 * r * r;
	}
}
```

If we have a circle object we can access member variables and methods with the `.` operators:

```cpp
Circle c;
c.r = 5.0;
std::cout << c.getArea() << "\n";
```

If instead we have a pointer to a circle object we use the `->` operator.

```cpp
Circle *c;
c->r = 5.0;
std::cout << c->getArea() << "\n";
```

Along with classes we get Encapsulation, which allows methods to provide a clear and stable interface and no external access to the implementation.
It's therefore easier to change the implementation and less error prone.

Classes also have `private` and `public` and constructors:

```cpp
class Circle {
private:
	double x, y, r;
	
	...
	
public:
	Circle(double x, double y, double r) {
		this->x = x;
		this->y = y;
		this->r = r;
	}
	
	...
};
```

There's a slightly better way to write the initialiser:

```cpp
public:
	Circle(double x, double y, double r) :
	    x{x}, y{y}, r{r} {};
```
- This gives values to member variables before the object is officially created
- Less boilerplate
- We can initialise constants, references and the base class this way.


if we had:
```cpp
	...
	const double r;
	...

public:
	Circle(double x, double y, double r) {
		...
		this->r = r; // ERROR cannot modify const
	}
```

Say if we had a class called Point
```cpp
class Point {
private:
	double x, y;

public:
	Point(double x, double y) : x(x), y(y) {};
};
```

We can now use our newer method of creating a constructor:
```cpp 
class Circle {
private:
	Point point;
	const double r;
	
public:
	Circle(double x, double y, double r) :
	    point(x, y), r{r} {};
	
	...
};
```

You can mess around with this including different types of constructors with different parameters and constructors can call constructors...

```cpp
Circle(double x, double y, double r) :
    Circle(Point(x, y), r) {};
    // this calls the constructor below!

Circle(Point point, double r) :
	point{point}, r{r} {};
```

Destructors

It's a special method that is automatically called when an object is destroyed.
1. Stack Allocated -> Object goes out of scope
2. Heap Allocated -> Call delete on the object.

The Destructor releases dynamically allocated resources such as memory, files and locks.

for example if Circle class had a dynamically allocated string called `name`:

```cpp
char* name;

void setName(char *name) {
	this->name = new char[strlen(name) + 1];
	strncpy(this->name, name, strlen(name));
}

~Circle() {delete[] name};
// destructor that takes care of the dynamically allocated string
```

##### Resource Allocation Is Initialisation (RAII)
Constructor
- Takes all dynamic resources needed: memory, files, etc...
- Initialises all class invariants
- Instance is ready to use right after declaration.
Destructor
- Releases all dynamically acquired resources
Resource Destructor Automatically called by
- Using resource through RAII-objects with automatic/temporary lifetime
- Bounding resource to the lifetime of an automatic/temporary object.

`new` needs a `delete` otherwise this causes a memory leak... Please `delete` in every possible point where lifetime ends.
- semantically determined points
- all return statements
- all exception handlers
But these are considered harmful so please try to avoid this as much as possible!

##### Operator Overloading
Can change what C++ operators can do for example:
```cpp
bool operator< (const MyInt& lhs const MyInt& rhs) {...}

void fn() {
	MyInt x{5}; MyInt y{4};
	if (x < y) {...} 
}
```

One quick note about structure, often we have a header file which includes only declarations and no implementations -although it can contain small methods, we can use `#pragma once` to ensure a header file is only included once in the compilation.

##### Inheritance
Very similar to python or java and abstracts away commonalities, We can add more data and code to the existing class and modify what the existing code does

Base class:

```cpp
#include “point.h”

class Shape{
private:
	int id;
	char* name;
protected:
  Point* points;

public:
  Shape(Point* points, int npoints);
  Shape(Point* points, int npoints, char* name);
  ~Shape();
  void setName(char *name);
  char* getName() {return name;}
```

And a derived class:

```cpp
#include “point.h”
#include “shape.h”

class Circle : public Shape {
private:
  double r;
public:
  Circle(Point point, double r);
  Circle(Point point, double r, char* name);
  double getArea();
  Point getCentre();
};
```

What is the difference between `private` and `protected`?
- `private` - visible only in ==class== code
- `private` - visible in this ==class and derived classes==!

##### Polymorphism
Multiple implementations for a given name.
Compile Time:
- Implementation known at compile time
- Compiler issues a direct call to the right implementation
- Function overloading
Runtime:
- Implementation not known at compile time
- Can change over lifetime of program
- Compiler issues indirect call

A common example of this is where a base class has a method, possibly `getArea()`, and our derived classes can override this class with their respective implementations.

We can do this by within our base class we do:

```cpp
virtual double getArea();

double Shape::getArea() {
	return 1.0;
}
```

The `virtual` keyword here is required if we're performing runtime polymorphism.

And in our derived class:

```cpp
double getArea() override;

double Circle::getArea() {...}
```

Virtual Functions have a runtime overhead, extra code has to be executed, requires more memory accesses which could be substantial. You can either pay the cost if you need runtime polymorphism or avoid the overhead by the default compile time polymorphism.


### For the Exam

What's one difference between `new` and `malloc`? Well when you use `new`, the memory it allocates will also all be set to `0` whereas `malloc` simply allocates the memory and does not reset the memory locations..

Remember that in C++, the `struct` keyword is now optional when referring to a instance of a struct.

Remember that in order to make a reference (`&`), that it must be initialised with a variable and can never be NULL and not point to anything - and it can never change what it's pointing to. AND it must be the same type!

