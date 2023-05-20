[[COMP26020]]

Right let's see if we can break Rust like you can with C...

```rust
fn swap(x : &Vec<i32>, y : &Vec<i32>)
-> (&Vec<i32>, &Vec<i32>) {
	return (y, x);
}

fn main() {
	let a = vec![6, 5];
	println!("{}", a.len());
	
	let x: &Vec<i32>;
	{
		let b = vec![7, 8, 9];
		let y : &Vec<i32>;
		(x, y) = swap(&a, &b);
		println!("{}", y.len());
	}
	
	println!("{}", x.len());
}
```

If we wanna break the language we hope that this compiles, why is this bad? Because if the `swap` is successful, then `x` is a borrow of `b`, but after the `{...}`, `b` becomes out of scope and is deallocated, so the line `println!("{}", x.len());` will be printing a deallocated value, so in truth we would want the compiler to catch this...

And.. it does not compile.. and the error is that, Rust does not know the lifetime of the two inputs `x` and `y` are for the `swap` function. AND it does not know the lifetime of the two outputs are either...


```rust
fn swap <'m, 'n>(x : &'m Vec<i32>, y : &'n Vec<i32>)
-> (&Vec<i32>, &Vec<i32>) {
	return (y, x);
}
```
So here we're saying the function has two lifetimes `'m`  and `'n`, and we give each input parameter a lifetime, here we have given `x` a lifetime `'m` 

And we should give the returned variables a lifetime (and be careful because getting this wrong will result in an error of `lifetime mismatch`)

```rust
fn swap <'m, 'n>(x : &'m Vec<i32>, y : &'n Vec<i32>)
-> (&'n Vec<i32>, &'m Vec<i32>) {
	return (y, x);
}
```
Mixing up the lifetime return values here will result in an error!

So after trying to write this function to out-smart Rust, we still get the error:
```rust
fn swap <'m, 'n>(x : &'m Vec<i32>, y : &'n Vec<i32>)
-> (&'n Vec<i32>, &'m Vec<i32>) {
	return (y, x);
}

fn main() {
	let a = vec![6, 5];
	println!("{}", a.len());
	
	let x: &Vec<i32>;
	{
		let b = vec![7, 8, 9];
		let y : &Vec<i32>;
		(x, y) = swap(&a, &b);
		println!("{}", y.len());
	}
	
	println!("{}", x.len());
}
```
This won't compile because `x` borrows `b`, but `b` does not live longer than `x`, as `b` here goes out of scope after the `{...}`, so we cannot actually trick the compiler like we can with C.


Overall:
- In Rust, functions should have their inputs and output's lifetime stated, so the compiler can keep track of them. This means most of the time the compiler can infer lifetimes but if it cannot infer lifetimes we will have to specify the lifetime.