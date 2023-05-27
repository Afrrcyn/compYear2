### Error Values

```haskell
x = x

print(x)
```
> Printing this would run forever.. The value which represents the program running forever is referred to as `bottom`, people think of ordering all values of a data type by how informative they are and bottom is the least informative. 

So it could have been defined as:
```haskell
bottom = bottom
```

```haskell
badhead :: [Int] -> Int
badhead (x:l) = x

uhoh = badhead []
main = print (uhoh) --- >>> Returns error
```
> Non-Exhaustive Patterns in function badhead

Fixed Code:
```haskell
safehead :: [Int] -> Maybe Int
safehead [] = Nothing
safehead (x:l) = Just x

headList = safehead []
main = print (headList)
```

Let's look at some more...
```haskell
eInt :: Int
eInt = error "Hang on..."

eBool :: Bool
eBool = error "Hang on.."

main = print (eBool)
```
> Used for printing out error messages and stuffs - however we're strongly advised against using the error expression `error "..."`, as a way of dealing with errors... Just fix your code.


```haskell
f x =  x + 3

main = print (f eInt)
```
> Simply causes an error but not all functions will have this error - look at code below

```haskell
g x = 5
main = print (g eInt)
```
> Now it outputs `5`, as function `g` does not evaluate it's parameter and therefore the error does not occur. P.S will always output 5 as parameter is never used.

>[!info] Strict
>We can say that `f` is strict in it's argument, because if it's argument is an error than so is `f` and therefore `g` is non-strict..

We can use this information to check whether for example if multiplication is strict in it's arguments.
```haskell
print(0 * eInt) -- ERROR!
print(eInt * 0) -- ERROR!
```

--------------------------------------------

### Reasoning with Errors

```haskell
main = print ((error "left" * error "right"))
```
> prints `error: left`, and switching the operands around will print `error: right` and thus it will appear as if we have broken the rule of commutativity.

> Note:
> -  Errors and Undefined Values are actually runtime errors and depends on the compiler and such, however it is still commutative - look it up if you don't believe me.


```haskell
stimes :: Int -> Int -> Int
stimes 0 n = 0
stimes m n = m * n
```
> Well `stimes` is just a multiplication operator so we may wanna use it as an infix operator - Haskell allows for this:

```haskell
main = print (0 `stimes` eInt) -- WORKS
main = print (eInt `stimes` 0) -- FAILS 

```
> The definition of our function is made to look at the left operand first and thus the second line will fail with an error, now I hear you ask, why not add another base case: `stimes n 0 = 0`, well - we cannot. Because when checking the first case - `stimes 0 n = 0`, we have to evaluate the first argument to check if it's `0` and so we evaluate `eInt` and this causes an error.

The fact that we cannot have multiplication be strict on both arguments is due to the inner workings of Haskell and is called ***Sequentiality***, which means any basic Haskell calculation can be done on a single thread - it can be possible to use more than one thread but basic calculations can be done on a single thread.

Logical Operations in Haskell are also defined in this way! And thus non-commutative when you consider all possible values (which includes errors)

Let's look at these observations with a more complex data structure - like pairs:
```haskell
strangePair = (eInt, eBool)
ePair = error "Hang on..."
```
> Remember that `eInt` and `eBool` are both defined in the same manner as `ePair`, so the question is, are they the same?

```haskell
h ::  (Int, Bool) -> Int
h (x, y) = 42 
```
> The function observes that it's parameter is a pair but it does not do anything with this pair.

and..
```haskell
j :: (Int, Bool) -> Int
j p = 10
```
So how will `ePair` and `(eInt, eBool)` behave with these functions `h` and `j`

```haskell
print (j strangePair)
print (j ePair)
```
> `j` treats both data values in the same way and returns `10`.

```haskell
print (h ePair)       -- ERROR
print (h strangePair) -- NOT ERROR
```
> the reason is because `h` is asking what are the components of the pair whereas `j` does not care. We can say that `h` is strict in the shape of it's arguments (some may say strict in the spine (same thing!))

-----------------------------

### Infinite Data

```haskell
data MyIntList = Emp | Cons Int (MyIntList)

v1 = Cons 10 (Cons eInt Emp)
v2 = Cons 10 (error "HI")

myHead :: MyIntList -> Maybe Int
myHead Empty = Nothing
myhead Cons x l = Just x

print (myHead v1)
print (myHead v2)
```
> Both of these lists actually work with function `myHead` this is due to the fact it `myHead` simply looks at the head and does not care about the tail - so we can say that `myHead` is strict in the head of the list but non strict in the tail of the list.
> 
> Note that: Haskells own list implementation is has the same behaviour

Let's try this function:
```haskell
myLen :: [Int] -> Int
myLen [] = 0
myLen (x:l) = 1 + myLen(x)

print (myLen [eInt, eInt, eInt]) -- WORKS
print (myLen (1:(error "Hi")))   -- ERROR
```
> So `myLen` is actually strict in the spine of the list (strict in the shape of the list) and so if it finds an element that is not in the shape of a list, it will return an error - this is because the definition of `myLen` uses pattern matching and fails when the list is actually an error. 

So we can actually defines a list recursively - much like you can define a function recursively.
```haskell
ones = 1:ones
```

> [!NOTE] 
> - There is a function called `take n [...]`, it takes an `Int n` and a list `[Int]` and will return a list of size `n` with the first `n` values in the original list `[...]`
> - There's another function called `drop n [...]`, which instead drops the first `n` items in the list and returns the rest of the list.

and thus: 
```haskell
ones = 1:ones
print (take 10 ones)
```
> This allows us to make an infinite data structure and only use the part we need. And because of the way Haskell works, it means it will only generate the part it needs, so it makes code more modular.

We can also define natural numbers
```haskell
nats = 0:[ n + 1 | n <- nats]
main = print (take 5 nats) -- >>> [0,1,2,3,4]
-- test with:
main = print (nats !! 7) -- >>> outputs: 7
```

>[!Info] Syntax
>- `list !! n`
>- This asks for the index of `list`, bearing in mind that Haskell is 0-indexed.

```haskell

fibb = 1:1[(fibb !! n) + (fibb !! (n + 1)) 
 | n <- [0..]]
```
> Recursive Definition of fibonacci numbers using a list comprehension.

Just a cooL implementation of primes.
```haskell
sieve :: [int] -> [int]
sieve [] = []
sieve (x:l) = x:[y | y <- sieve l, 
 y `mod` x /= 0]

primes = sieve [2..]
```

We can only use infinite data structures because some functions are not strict and therefore only care about some of the inputs and not all of the inputs and therefore we can work on infinite data sets without worrying about the whole set.