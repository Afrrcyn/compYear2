[[COMP26020]]

We can express algorithms and data structures in terms of generic types and those templates can be instantiated for specific types at compile time - instead of wiring specific algorithms for all types - you can already see why this is cleaner.

```cpp
int min(int& x, int& y) {
	if (x < y) return x;
	return y;
}

float min(float& x, float& y) {
	if (x < y) return x;
	return y;
}
```

Here we wrote to almost identical functions the only difference being the return value and parameter types.. how about instead:

```cpp
template<typename T>
T min(T& x, T& y) {
	if (x < y) return x;
	return y;
}
```
Exactly the same however now it works for all types!

```cpp
int main() {
	int x = 4, y = 5;
	float xf = 4.0, yf = 5.0;
	min(x, y);   // WORKS!
	min(xf, yf); // WORKS!
}
```

And this function will work for any type that has the `<` operator defined for it, as the function uses the `<` operator `if (x < y) ...`, so we can even create our own class and override what the `<` does for our liking:

```cpp
min(MyInt(4), MyInt(23)); // should work in theory
```

Besides functions we can also put generics within classes as well.

Python uses type erasure and C has macros, but C++ seems to be the best one here -  please do look up Type Erasure for more information it's prolly gonna come up on the exam!

Macros
- Are just a textual substitution
- Have no type safety
- Error Prone
- Hard to read, even for short macros
- Work only for simple problems

>[!note] Templates are turing complete!

### C++ Standard Lib

The core design of the C++ standard library is as follows:

![[Pasted image 20230328170251.png|500]]

We'll be going through each of these now:

##### Containers
Collections of data organised in some standard structure
- Templated classes
- The data type is a template parameter
- All data have the same type
- Almost all types can be used

Different implementations, interfaces and trade-offs
Used generally through a common abstraction

![[Pasted image 20230328171119.png|500]]

We will focus only on those that are examinable:

`std::vector<T>`
- variable size array of type T
- encapsulated heap array of T
- similar to Python list or Java ArrayList but far more efficient

```cpp
template<typename T>
class vector {
	T* data
	size_t size;
}
```

```cpp
#include <vector>

std::vector<int> v1; // size = 0
std::vector<int> v2(10); // size = 10
std::vector<int> v3{0, 1, 2, 3, 4, 5}; // size = 6

v3.push_back(6);
v3[2] = 1;
v3.at(3) = 2; // same as v[3] = 2;
v3.at(10) = 5; // exception
```


`std::array<T, k>`
- Fixed size array of size K and type T
- Encapsulated stack array of T
- Safe and fits in the library framework

```cpp
template<typename T>
class array {
	t data[k];
}
```

```cpp
#include <array>
std::array<int, 5> arr1; // size = 5, {0, 0, 0, 0, 0}

std::array<int, 3> arr2{0, 1, 2}; // size = 3

arr2.push_back(6); // Not valid operation
arr1[2] = 1;
arr1.at(4) = 2; // same as arr1[4] = 2;
arr2.at(4) = 0; // exception
```

![[Pasted image 20230328171833.png|700]]

We also have
`std::unordered_set` = python set
`std::unordered_map` = python dict

```cpp
#include <set>
std::set<int> s1;

std::set<int> s2{1, 5, 7, 2};

s2.insert(3);
s2.erase(2);
auto it = s2.find(1);
size_t count_five = s2.count(5);
```

```cpp
#include <map>

std::map<int, int> powers2;
powers2.insert({5, 32});
powers2.insert({10, 1024});
powers2.insert({11, 2048});
powers2.erase(10);

auto it = powers2.find(11);
size_t count_five = powers2.count(5);

powers2[12] = 4096; // creates 12 → 0, then sets 12 → 4096
int val = powers2.at(13); // exception
```

There are 2 types of associative containers...

Ordered
- Elements have to be comparable.They are kept in some order.
Unordered
- Elements have to be hashable. They are kept in a hash map, this is faster but needs more space...

`std::span` almost a python slice [start:end]
```cpp
#include <span>

...
int a[]{1, 2, 3, 4, 5};
std:span s{a};
std::cout << s.size() << "\n"; // prints 5
```

All of these have a common interface!

```cpp
empty()
size()
max_size()
clear()
insert()
emplace()
erase()
begin()
end()
cbegin()
cend()
```

Next up..

##### Iterators
- With pointers we can access data, increment, decrement.
- This only really works with arrays as a pointer is just a memory location and arrays have data that is stored contiguously.

One thing to note - which you may have sene before is `auto`, we may not always need to say what variable type we are expecting, instead we can type `auto` and the compiler can do the work for us!

```cpp
std::vector<int> c1;
for (
std::vector<int>::iterator it = c1.begin(), // get begin iterator
std::vector<int>::iterator last = c1.end(); // get end iterator

it != last;

++it) {
    // do something with *it
}
```

Here the iterator is a generic pointer, for which the behaviour is defined and we do not need to worry about it.

But remember `auto`:

```cpp
std::vector<int> c1;
for (
auto it = c1.begin(), // get begin iterator
auto last = c1.end(); // get end iterator

it != last;

++it) {
    // do something with *it
}
```

Is this truly the best way to loop through a vector?

>[!info]- Answer:
>neh -  you could prolly see that by the code below...
>

```cpp
std::vector<int> c1;

for (auto& elem c1) {
	// please do something with elem 
}
```

There's also `Ranges`

```cpp
template <typename R, typename T>
auto find(R& range, T& value) {
// find value
	auto first = ranges::begin(range), last = ranges::end(range);
	while (*first != value && first != last)
		++first;
	return first;
}
```


##### Algorithms
- There are so many algorithms, some that use iterators (generic pointers) and others that use ranges (std::ranges::)
- Some are non-modifying
	- find, find_if, for_each, all_of, any_of, none_of
- Modifying
	- copy, move, swap, fill transform, reverse
- others..
	- sort, is_sorted, binary_search, max, min, iota, accumulate, reduce

### Some other bits of the library

`std::string`
- encapsulates a C string
- Similar interface to that of `std::vector<char>` -> `[]`, `at()`, `push_back()` ...
- String specific interface -> `c_str()`, `substr()`, `find()`, stream and comparison ops


`std::format`
- python like formatting
```cpp
#include <format>
std::cout << std::format(“Hello {}\n”, “World”);
std::cout << std::format(“Pi is: {0:.1f}”, 3.1415);
```

`iostream`
- provides high level I/O abstraction
- `std::cin`, `std::cout` and `std:cerr`
- formatted I/O uses `<<` and `>>`
- Unformatted I/O uses `get`, `getline`, `read`, `put` and `write`
- Position functions `tellg` and `seekg`
- Others include: `flush`, `sync`, `good`, `fail`, `eof`

`fstream`
- High level I/O abstraction for files

```cpp
#include <fstream>

{
	std::ifstream infile("nums.txt");
	std::vector<int> v;
	int num;

	// read one number into num
	while (infile >> num) {
		v.push_back(num);
	} // file closed automatically RAII
}
```

`filesystem`
path() → path representation (similar to Python Path()) 
- / - append path to path  
- Decomposition functions  
- Comparison operators
directory_iterator() - Iterate over all directory files
Utilities  
Create, copy, rename, remove, file info..

`chrono`
- Date and time utilities

```cpp
#include <chrono>
// Current point in time using the most accurate clock
auto start = std::chrono::high_resolution_clock::now();
// Do whatever you wanted to do
fibonacci(42);
// Current point in time using the most accurate clock
auto end = std::chrono::high_resolution_clock::now();

// Get the distance of the two points and print it as seconds
std::chrono::duration<double> elapsed_seconds = end - start;
std::cout << "elapsed time: " << elapsed_seconds.count() << "s\n";
```