[[COMP26020]]

What is functional programming?
- treat functions like any other data
	- define without naming
	- functions as inputs and outputs of other functions
- Reason about them like mathematical functions
	- if $fx = gx$ for all $x$ then $f = g$

C++ is not a functional language! It may treat functions like data but you cannot reason about them however it does have some functional capabilities.

The functions:
- `transform()` apply function on range and store in another range
- `copy_if()` copy elements in range to another range if they make a predicate true.
- `reduce()` reduce the range into a single value by applying a function on every element

All of these are functions that take functions as inputs!

`transform` is equivalent to a Functional Programming `map`
`copy_if` is equivalent to a FP `filter`
and `reduce` a `fold`

### lambda

We also have Lambda Expressions now!
`[capture](params) -> return_type { body }`
`[] (auto x, auto y) {return x > y ? x : y }`

- Can be passed as arguments
- Can be returned from functions
- Can be stored
- First Class Citizens

Okay but what the heck is that `[capture]`?? Good question.. It captures part of the environment, binds names outside the lambda to names inside.

```cpp
int threshold = 10;
std::vector<int> v;
...
auto it = std::find_if(v.begin(), v.end()),
	[threshold](int x){ return x > threshold});
```

`[]` - Capture nothing  
`[var]` - Capture var by value  
`[&var]` - Capture var by reference  
`[=]` - Capture everything by value  
`[&]` - Capture everything by reference  
`[&, var]` - Capture by reference, apart from var

### Range VS Range View?

This is a range implementation:

```cpp
// Find the first 8 Mersenne (2^n – 1) primes
std::vector<int> output;
std::vector<int> v(63); // Is n <= 63 enough?
std::ranges::iota(v, 1); // 1, 2, ..., 63

// Get 2^n – 1 for all 63 numbers, v = 2^v - 1
std::ranges::transform(v, v.begin(), [](int x) {return (1<<x) – 1;});
// Back insert copies of prime numbers in output
std::ranges::copy_if(v, std::back_inserter(output), is_prime);

// Keep the first 8 numbers
output.resize(8);
```

Here's a Range View Implementation:

```cpp
// Find the first 8 Mersenne (2^n – 1) primes
// View of all natural numbers
std::views::iota v(1); // 1, 2, 3, 4, 5 ...  
// View for an infinite sequence of 2^n – 1 numbers
auto v2 = std::views::transform(v, [](int x) {return (1<<x) – 1;});
// View of an infinite sequence of Mersenne Primes
auto v3 = std::views::filter(v2, is_prime);
// View of the first 8 Mersenne Primes
auto v4 = std::views::take(v3, 8);

// Evaluate here and construct the output vector
std::vector<int> output(v4.begin(), v4.end());
```

And this can be made cleaner :

```cpp
auto v = std::views::iota(1) |
         std::views::transform([](int x) {return (1<<x) – 1;}) |
         std::views::filter(is_prime) |
         std::views::take(8);

// Evaluate here and construct the output vector
std::vector<int> output(v.begin(), v.end());
```

This is actually about x2 faster whilst being more clear and concise, using far less space!

### `new`

The new keyword is considered harmful remember..
- We have to manually allocate and deallocate
- Causes memory leaks if not
- And what if we use it after we free the memory??

So what do we do? Well we use containers and most of the allocation is done for us, and cleaned up for us.

##### Smart Pointers
How can we create objects with dynamic types?

```cpp
 Base b;
 Derived d;
 Base *ptr = (condition) ? &b : &d;
 // PROBLEM:
 // creates both objects
```

hmm what about

```cpp
{
	Base *ptr = (condition) ? Base() : Derived(); 
	// ERROR, pointer to rvalue
}
```

We could do

```cpp
{
	Base *ptr = (condition) ? new Base : new Derived;
	// WORKS but now we're using that pesky *new*
}
```

Let's introduce smart pointers

- Encapsulates a raw pointer to heap memory and uses the same syntax
- Has custom constructors and destructors to manage lifetime
- `std::unique_ptr<T>`
- `std::shared_ptr<T>`
- `std::weak-ptr<T>`

![[Pasted image 20230328205634.png|600]]

```cpp
{
auto sptr = (condition) ? std::make_unique<Base>() : std::make_unique<Derived>();
}
```

Now that's hot ^

### Null
So uh, turns out, Null is also bad...
Yeah there's no such thing as Null, it's simply a preprocessor directive and Null is replaced with 0.. now if you do `ptr != NULL` this is a comparison between pointer and an int which does not makes sense.
Null is also bad because it can be redefined..

Well we have `nullptr` that works out of the box!

### Multiple returns

so earlier we solved multiple returns by using a reference in the parameters of the method but this can also make the function look messy and unclean - also very unclear on what is an input parameter and what is an output parameter. we can solve this as well with `std::tuple<Type1, Type2, Type3, ...>` or `std::pair<Type1, Type2>`

```cpp
#include <pair>

...

std:pair<int, int> func() {
	int val1, val2;
	....
	return {val1, val2}
	//or 
	std::make_pair(val1, val2);
}

auto [ret1, ret2] = func();
```

This is much cleaner now!

### Contextexpr

Can speed up runtime by performing operations at compile time that will never change.

- Value is constant and can be initialised at compile time
- Function returns constant values for constant inputs
- Compiler calculates parts of the program at compile time
- Less code, faster execution
```cpp
contextexpr int fibonacci(int num) {  
if (num == 0)
        return 0;
    if (num == 1)
        return 1;
    return fibonacci(num - 1) + fibonacci(num - 2);
}
```


Finally, remember all of these rules they can come up!
### REMEMBER:
P.1: Express ideas directly in code  
P.2: Write in ISO Standard C++  
P.3: Express intent  
P.4: Ideally, a program should be statically type safe  
P.5: Prefer compile-time checking to run-time checking  
P.6: What cannot be checked at compile time should be checkable at run time P.7: Catch run-time errors early
P.8: Don’t leak any resources  
P.9: Don’t waste time or space  
P.10: Prefer immutable data to mutable data  
P.11: Encapsulate messy constructs, rather than spreading through the code P.12: Use supporting tools as appropriate  
P.13: Use support libraries as appropriate
F.4: If a function might have to be evaluated at compile time, declare it constexpr  
F.21: To return multiple "out" values, prefer returning a struct or tuple  
C.3: Represent the distinction between an interface and an implementation using a class  
C.9: Minimise exposure of members  
C.33: If a class has an owning pointer member, define a destructor  
C.49: Prefer initialisation to assignment in constructors  
C.51: Use delegating constructors to represent common actions for all constructors of a class  
C.132: Don't make a function virtual without reason  
C.160: Define operators primarily to mimic conventional usage  
R.1: Manage resources automatically using resource handles and RAII (Resource Acquisition Is Initialisation)  
R.3: A raw pointer (a T*) is non-owning  
R.10: Avoid malloc() and free()  
R.11: Avoid calling new and delete explicitly  
R.20: Use unique_ptr or shared_ptr to represent ownership  
ES.1: Prefer the standard library to other libraries and to "handcrafted code"  
ES.11: Use auto to avoid redundant repetition of type names  
ES.23: Prefer the {}-initialiser syntax  
ES.47: Use nullptr rather than 0 or NULL  
SF.1: Use a .cpp suffix for code files and .h for interface files if your project doesn't already follow another convention 
SL.con.1: Prefer using STL array or vector instead of a C array  
SL.con.2: Prefer using STL vector by default unless you have a reason to use a different container