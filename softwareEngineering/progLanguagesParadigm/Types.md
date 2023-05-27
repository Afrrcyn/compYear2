Checked at compile time and every expression will always have a type.
```haskell
f :: Int -> Int
f x = x + 1

print(f 10)
print(f (10::Double))
```
> `f` is a function from `Int` to `Int` - so the first print function will print `11` (note that, it won't be printed here because the second print function causes a compile error!) whereas the second will cause an error as `f` is expecting an `Int` and `10` is being cast to a `Double`

We also have type `Integer` - which is unbounded and if you had enough memory you could store any number with Integer.

> In Haskell, `Int` and `Integer` are both types used to represent integers, but they have some differences in their range and performance characteristics. 
> `Int` is a fixed-precision integer type that is usually implemented as a 32- or 64-bit signed integer depending on the platform. This means that `Int` has a limited range of values that it can represent, which is typically from -2^31 to 2^31-1 or from -2^63 to 2^63-1. Because `Int` has a fixed size, arithmetic operations on `Int` are generally fast, and `Int` is suitable for most applications that do not require arbitrary-precision arithmetic.
> `Integer`, on the other hand, is an arbitrary-precision integer type that can represent integers of any size. This means that `Integer` can represent values that are much larger (or much smaller) than the range of values that `Int` can represent. However, because `Integer` uses a variable amount of memory to represent each value, arithmetic operations on `Integer` can be slower than on `Int`. `Integer` is typically used when exact precision is required, such as in cryptography or symbolic computation.

```haskell
big :: Int -> Bool
big n = x > 10
```
> Simply showing off the type of `Bool`

We do not have to explicitly state a type as the compiler will infer the type based on the usage.

Have a look at this though....
```haskell
j x = False

main  = print (j 10 && j False)
```
> This will not cause an error and actually returns `False`, but how can the compiler infer the input type from this code? So, `j` is a generic function and allows for many types of inputs because the compiler is a big brain.

We can state this ourselves if we want to:
```haskell
j :: a -> Bool
j x = False

main = print (j 10 && j False)
```
> Note that the generic type must be lower case (`a`)!

--------------------------------------------

### Type Constructors
A way to compile new types.

```haskell
sumPair :: (Int, Int) ->  Int
sumPair (x, y) = x + y
```
> This expression `(x,y)` is a pair and this: `(Int, Int)` is a pair of types

```haskell
number :: Int
```
> The line above can be translated to plain english `number is an element of the set Int`

```haskell
atTen :: (Int -> Bool) -> Bool
atTen f = f(10)
```
> The function `f` is defined at as `(Int -> Bool)` and `atTen` takes a function and maps it to a `Bool`

```haskell
myApp :: (Int -> Bool) -> Int -> Bool
myApp f x = f x
```
> The first Argument `(Int -> Bool)` is the types for the function `f`, the second argument is the input type for `x` and so looking at the interface for function `f`, it's wanting a type `Int`, and the Third argument is the return type of function `f`, which is `Bool`.
> 
> So to generalise it:
> `(a -> b) -> a -> b`, where all `a`'s must be the same type and all `b`'s must be the same type.

--------------------------------------

### Algebraic Datatypes

We can make our own types using the `data` key word with the simplest type to make being an enumeration:
```haskell
data SwitchState = On | Off

isOn :: SwitchState -> Bool
isOn On = True
isOn Off = False

toggle :: SwitchState -> SwitchState
toggle On = Off
toggle Off = On
```
> Not that if we tried the line `print (toggle On)`, this would cause an error with the Haskell compiler as the type `On` is newly defined and Haskell does not yet know how to print it. 
> Therefore, we have to use `print (isOn(toggle On))`

```haskell
data MyIntPair = IntPair Int Int

mySumPair (IntPair x y) = x + y

main = print(mySumPair(IntPair 10 11))
```
> Note the syntax, we have the constructor name `IntPair` and it's arguments `Int` and `Int`


```haskell
data BoolOrInt = Abool Bool | Anint Int

intVal :: BoolOrInt -> Int
intVal (Abool True) = 1
intVal (Abool False) = 0
intVal (Anint n) = n
```
> This is very useful for error handling

```haskell
data MaybeInt = NoInt | JustInt Int
```
> Note that `NoInt` does not have a data field and can just be used to check if the value is not of type `Int`

```haskell
data MyPair a b = Pair a b

myFirst (Pair a b) = a

main = print (myFirst(Pair "Hey" 10))
```
> Infers the data field based on the parameters.

Let's look at this:
```haskell
Maybe a = Nothing | Just a
```
> Defined by default in Haskell

We can also use recursive definitions:
```haskell
data MyList a = Empty | Append a (MyList a)

myHead :: MyList Int -> Maybe Int
MyHead Empty = Nothing
--Myhead (Append x (Empty)) = Just x
--Myhead (Append x (MyList n)) = Just x
Myhead (Append x xs) = Just x
```
> Very much like a recursive definition of a list, that we saw in Year 1 Maths, the reason we use a `Maybe Int` for the output is due solely to the fact that if the list is empty, we cannot return an int.

--------------------------------------

### List Syntax

```haskell
myIntHead :: [Int] -> Maybe Int
myIntHead [] = Nothing
MyIntHead x:l = Just x

main = print (myIntHead (1:2:3)) >>> Just 1
main = print (myIntHead [1,2,3]) >>> Just 1
main = print (myIntHead "Hi!") >>> Just 'H' -- (why does this happen? because string is simply a list of charactersp)
```

>This is how lists are done, notice that both print statement are syntactically correct, let's make this a little smarter:

```haskell
main = print ([1..5]) >> [1,2,3,4,5]
main = print ([1,3..10]) >> [1,3,5,7,9] -- so upto 10 with the same difference as from 1 to 3
```

```haskell
myHead :: [a] -> Maybe a
myHead [] = Nothing
MyHead x:l = Just x
```
> Now it's more generic!

>[!Info] Note That
>- String `"hello"`, is simply just a list of characters! `"hello"::[Char]` or `"hello"::String` mean exactly the same thing.
>- Writing `[1, 2, 3, 4, 5]` can take a while instead we can write `[1..5]` however please do not that `[1, 3..10]` is not `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]` but rather `[1, 3, 5, 7, 9]` where the difference of the first two numbers `1` and `3` determines the difference between the rest...

List Comprehension:
```haskell
l = [(w, n) | w <- "Hi!", n <- [1..3]]
-- prints:
{-- 
('H', 1), ('H',2), ('H',3)
('i', 1), ('i',2), ('i',3)
('!', 1), ('!',2), ('!',3)
--}
```
> Note the order.., it's like a for loop, the below code shows the python equivalent.

```python
l = list()
for w in "Hi!":
	for n in range(1, 4):
		l.append((w, n))
	
```

You can add further conditions:
```haskell
l = [(w, n) | w <- "Hi!", n <- [1..3], w /= 'i']
-- prints:
{-- 
('H', 1), ('H',2), ('H',3)
('!', 1), ('!',2), ('!',3)
--}
```
