[[COMP26020]]

In C we can manually use `malloc` and `free`, but this has it's own problems...

- Use after free - using a variable after freeing it can lead to ==undefined behaviour==
- And forgetting to free, leads to a ==memory leak==

For Java, it has a 'garbage collector' which solves these problems but at the cost of efficiency...

- and in Java we can still get Null Pointer Exceptions and so there are ==still clearly some memory management issues==.. 
- And these occur when two variables refer tot he same data value - called ==aliasing==
- One variable can be used to modify the data in a way which is unexpected fro the intended use of the other.

Rust takes it's own approach, where the compiler takes charge and ensures this never happens - BUT checking whether a C program has a memory fault is uncomputable..? Well, Rust ensures that the programmer has to code in a restricted way with annotations - and these restrictions are known as ==Rust's Ownership System==

In Rust every value of a reference has an owner - which is a variable and ==Rust ensures that:==
1. Every value has only one owner
2. The data is deallocated if the owner goes out of scope.
3. Assignments and Function calls pass ownership between variables.

```rust
fn main {
	let s String::from("Hello");
	println!("{}", s);
}
```
Works!

```rust
fn main {
	let s String::from("Hello");
	let x = s;
	println!("{}", x);
}
```
This also works!


```rust
fn main {
	let s String::from("Hello");
	let x = s;
	
	println!("{}", s);
	println!("{}", x);
}
```
This does not work! As we have passed ownership to `x` and so `s` does not own the value and so it cannot print it...

What about this:
```rust
fn main {
	let s String::from("Hello");
	
	myprint(s);
	
	println!("{}", s);
}

fn myprint(s : String) {
	println!("{}", s);
}
```
Doesn't work... the function call also transfers ownership.. and so `s` within the function `myprint` now owns the string and it goes out of scope after the function has been ran and so the string is then deallocated...

One solution is to return the string in the function..
```rust
fn main() {
	...
	let s = myprint(s);
	...
}

fn my myprint(s : String) -> String {
	...
	return(s);
}
```
This does indeed work but is actually very annoying...


So there's something in Rust called borrowing - so a way of referring to a value without taking ownership of it. In Rust you prefix this with `&`


```rust
fn main() {
	let s = Sting::from("hello");
	
	let x = & s;
	let y = & s;
	
	println!("{}", x);
	println!("{}", y);
	println!("{}", s);
}
```
WORKS

And let's look at the `myprint` code that was broken.

```rust
fn main {
	let s String::from("Hello");
	
	myprint(s);
	
	println!("{}", s);
}

fn myprint(s : & String) {
	println!("{}", s);
}
```
Here we have borrowed s, instead of taking ownership in the `myprint` function.

>[!question]- What is the macro `!` in `println` used for?
>It's to enable the borrow of the string, without explicitly stating `println("{}", & s);`, Why is this a thing? I do not know...

Please note that we only have to worry about borrowing for reference types - we do not need to worry about basic types like integer type `u32`, a value of this type is ==just copied== because it ==cannot change it's size== - it will be always 32 bits...

However there is another feature of Rust that applies to all variables in rust, and that is ==Mutability==.

Let's look at some code:

```rust
fn main() {
	let i = 10;
	let j = i;
	
	println!("{}", i);
	println!("{}", j);
}
```

>[!question]- Does this work? Think about borrowing..
>YES this does work, because when dealing with basic types that are not references and therefore do not change size, we simply copy the value instead of transferring ownership, so `j` has a copy of the value of `i`


```rust
fn main() {
	let i = 10;
	let j = i;
	
	i = 11;
	
	println!("{}", i);
	println!("{}", j);
}
```

This however does not work.. and we have to declare `i` as mutable to be able to change it.

```rust
	...
	let mut i = 10;
	... 
```

This enables us to change the value of `i`

Another fix which is referred to as `shadowing`

```rust
fn main() {
	let i = 10;
	let j = i;
	
	let i = 11;
	
	println!("{}", i);
	println!("{}", j);
}
```

Which is simply creating a new variable called `i`, and set it a new value..


This just seems like we're back at square one... if we can have many borrows of a  mutable variable.. it's the same issue with C and Java??

Well, when you borrow a variable - you say whether you're borrowing mutably or not, and you can ==either borrow immutably many times or borrow mutably once==

A borrow is active between it's creation and it's last use!

```rust
fn main() {
	let mut s = String.from("Hello");
	s.push_str(" world!"); 
	// ^ because mutable this works
	
	println!("{}", s);
}
```
All works fine...


```rust
fn main() {
	let mut s = String.from("Hello");
	s.push_str(" world!"); 
	
	let mut t = s;
	t.push_str(" and hello you.");
	
	println!("{}", t);
}
```
And this works as well.


```rust
fn main() {
	let mut s = String.from("Hello");
	s.push_str(" world!"); 
	
	let mut t = s;
	t.push_str(" and hello you.");
	
	println!("{}", s);
}
```
This however is wrong because `t` has ownership of the string and `s` does not anymore...

What about this now:
```rust
fn main() {
	let mut s = String.from("Hello");
	s.push_str(" world!"); 
	
	let mut t = & s;
	t.push_str(" and hello you.");
	
	println!("{}", s);
	println!("{}", t);
}
```

>[!Question]- Does this work?
>It does not, we have to let the borrow know that it's a mutable borrow - although we have stated that `t` is mutable this is not enough.. `let mut t = &mut s;` this fixes our issue.


Now I guess it's error free right?

- No, we can only have one mutable borrow at a time, and we do `println!("{}", s)` whist the mutable borrow of `t` is still live.. and so this will give us an error...

```rust
fn main() {
	let mut s = String.from("Hello");
	s.push_str(" world!"); 
	
 <- let mut t = & s;
 |	t.push_str(" and hello you.");
 |	
 |	println!("{}", s);
 ->	println!("{}", t);
}
```
The arrows above show the lifetime of `t` and you can see we use  `s` which is mutable, during the lifetime of the mutable borrow `t`, which will cause an error, the solution is simply moving the print of `s`, outside the lifetime of `t`


>[!question]- Is `t` in this context mutable: `let t = &mut s;`
>Whilst we're not explicitly stating it, because we have a mutable borrow, it means that `t` is actually mutable and we don't have to state it!

>[!question]- Can we have an immutable and mutable borrow of a reference happening at the same time?
>You cannot. You can have ***either*** many immutable borrows or just one mutable borrow happening at the same time

