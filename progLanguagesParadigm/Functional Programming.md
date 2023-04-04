One of the key features of functional programming is the treatment of functions much like any other piece of data.
- So to define them without naming them.
Other features are:
- I want to be able to pass functions as inputs and outputs within other functions - known as ***Higher Order Functions***
- We want to reason with functions as if they were mathematical functions.
	- Extensionality
	- Equality

>[!example] Example of Extensionality
>`if fx = gx for all x, then f = g`
>Why would we want to swap and replace functions?
>- well, what if a have a function that is obviously correct and a function that is much faster - if they turn out to be the same for all inputs and outputs, we can then swap to the faster one.
>- Note that this is possible because we have deterministic functions in functional programming that simply map one input to one output. (Haskells perspective!)

>[!example] Example of Equality
>`let x = f(10) in x + x + x` which is basically equivalent to `f(10) + f(10) + f(10)`
>- Remember this is very much possible because functions in functional programming simply map values from an input to an output deterministically.
>- We should not need to worry about the order of which the calculation is done, or whether the value is being calculated each call or simply copied.

--------------------------------------

### Basic definitions
Everything we write in Haskell is a definition.

Booleans and Logical Functions:
```haskell
b = True || (True && False)
```
> `b` equals True OR (True AND False)

```haskell
main = print (b)
```
> We do not need to understand this right now...

Comparators: 
```haskell
b1 = 3 == 4
b2 = 3 /= 4
```
> Our comparators, so `==` is the usual equality sign and `/=` is slightly different to that of your normal standard, but it's not equal.

Code over two lines:
```haskell
b = 4
 + 3
```
>We can have code over 2 lines - if when it continues onto the next line, the next line starts with a whitespace '` `'

```haskell
b3 = (0,0) == (0,1)
b4 = (0,0) == (0,0)
```

^1283c5

> `b3` is false and `b4` is true.

If-Then-Else
```haskell
v1 = if b then 5 else 6
v2 = 7 * (if b then 5 else 6)
```
> Means exactly what it looks like.

Defining a function:
```haskell
add7 n = n + 7
{-Test it out-}
main = print (add7 8)
```
>Simply defined a function - much like we defined a variable.

Further Definition of a Function
```haskell
small 0 = True
small 1 = True
{-All other numbers are not small-}
small n = False
```

^dea07f

> if the function `small` is called with values `0` or `1`,`True` is returned, however any other value, `False` is returned.
> Please also note that the order of these functions matters, if `small n = ...` was defined before `small 1 = ...` then `print (small 1)` would return `False` (and a warning!)

More than 1 parameter
```haskell
addUp (n, 0) = n
addUp (0, m) = m
addUp (n, m) = n + m
```

^a5a227

```haskell
first (e, _) = e
print (first (1, 2))
```
> the `_` here simply means to ignore that part.

Recursive Definitions:
```haskell
fib 0 = 0
fib 1 = 1
fib n = fib(n-1) + fib(n-2)
```

--------------------------------------

### Higher Order Functions

Let's define a function, that takes any input `n`, and then adds `m` to it.
```haskell
addConst n m = n + m
```
>Exciting stuff!
>- Also note, this is the most natural way of modelling multiple argument functions - not like the pair used [[#^a5a227|earlier]]!

BUT:
```haskell
addThree = addConst 3
```

There's another way to more explicitly show that a function returns a function:
```haskell
addConstB n = \m -> n + m
print (addConstB 3 7)
```
We could completely turn the function into an identifier.
```haskell
addConstC = \n -> (\m -> n + m)
```
> AddConst, takes a function that takes `n` and returns a function that takes `m` and returns `n + m`
> - Note that the interface in which we call the function has not changed with all these implementations.

So, we've looked at functions that return a function, let's look at functions that take functions as an input.
```haskell
thriceFromZero f = f(f(f(0)))

main = print (thriceFromZero(\n -> 2*n + 1))
```
> Defines a function that takes a function `f` and calls it 3 times with a parameter `0`


What if:
```haskell
h n = \n -> n

main = print (h 1 2)
```
> What will be printed? The function takes two things, `n` and `\n`, and it returns `n`, well it will return the most recent one, which is the parameter `\n`, so `h 1 2` will print `2` - where the most recent variable matters - is often called ***Shadowing***

^a6d609

Let's look at some higher order functions with recursion:
```haskell
repFromZero 0 f = 0
repFromZero n f = f(repFromZero (n-1) f))

main = print (repFromZero 3 (\x -> x + 1))
```
> Aim of this is to call the function `f`, `n` times - you can very clearly see the first line is the base case and the second line is the step case.

>[!bug] The above code will print `3`
>I am not sure how it prints `3`

--------------------------------------

### More Useful Forms of Definitions

Well instead of breaking a function over multiple lines based on cases we can:
```haskell
small n = case n of 
 0 -> True
 1 -> True
 n -> False
```
> This is the same version as [[#^dea07f|this]]


```haskell
sideOfFive n
 | n - 5 > 0 = 1
 | n - 5 < 0 = -1
 | n - 5 == 0 = 0
```
> Guard Expression - very much like an `if` statement, please also notice I write `n - 5` many times over and this is repetitive...

```haskell
sideOfFive n 
 | d > 0 = 1
 | d < 0 = -1
 | otherwise = 0
 where d = n - 5
```

Let Expression
```haskell
y = let x = 10 + 10 in x + x
```
> `y` is 40 here, and the `let` command is also have the abilities of [[#^a6d609|shadowing]], now what happens in the code below, what is printed when we print `w`?

```haskell
w = let x = 5 in 
 let f = \n -> x
 let x = 6 in
 f 0

main = print (w)
```

>[!info]- Answer
> `w` will equal `5` and that's because, the function is defined just after setting `x = 5` and once defined the function cannot really be changed.



### Stuff that goes wrong...

```haskell
oops True = True

main = print (oops False)
```
> Well, the function `oops` has not been defined for `False` and so the above code will print `True` when `False` is given by the parameter - 'Non-exhaustive Pattern Matching given by `oops`'

```haskell
eep 0 = 0
eep n = 1 + eep n
```
> Infinite Loop!


```haskell
yikes = let x = 1 in
 let x = 1 + x in x
```
> Remember that `Let` is just a local redefinition of a variable. This is almost like a recursive definition of a variable that infinitely loops...

This code below will also run forever
```haskell
yikes = let x = x in x
```
