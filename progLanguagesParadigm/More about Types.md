# More about Types

# Parametric vs Ad-Hoc Polymorphism

## Parametric Polymorphism

> Polymorphism: Having many forms
> 

```haskell
f :: a -> Bool -- function f takes a type variable 'a' and outputs a Boolean
f _ = True     -- always output True

main = print((f 7) && f (True)) -- see how we are passing two different types of data
																-- (f 7) - integer and (f True) - boolean, this done using
																-- type variable
```

### Type Variables

Type variables begin with a lowercase letter

From the example above we can see that we can pass in two different data type values (int, boolean) to get our output

`f :: a -> Bool`: This means that for all ($\forall a$) values of a, f takes a value and gives a boolean

`f _ = True` : This type of definition of the polymorphic function where the definition has to be the same for all possible types `_` is called **parametric polymorphism**

```haskell
g :: (b -> Bool) -> Bool
g h = (h 7) && (h True)

main = print (g f)
-- this returns an error of type b being rigid, why?
```

The problem here is that the forall ($\forall$) in polymorphic type signature comes outside of the whole type. 

`g :: (b -> Bool) -> Bool` This says that for all types of `b`, if you give a function from that type specific type to `Bool`, then `g` must output a `Bool`

> I do not really understand the second example, does he mean that the `b -> Bool` has to be `forall`? Which cannot be `forall` . Maybe what he means is the `Bool` in the input parameter cannot take the `7` that we are passing it into.
> 

```haskell
p :: (a,a)
p = (True, True)
main = print("")
-- this too returns an error, why?
```

We are giving in a pair of Booleans, is that why we are getting an error? What if we gave in a Boolean and an integer at the same time? Let’s try that out

```haskell
p :: (a,a)
p = (7, True)
```

## Ad-Hoc Polymorphism

> Sometimes called ‘overloading’
> 

Ad-hoc Polymorphism is when the same function name can actually mean different things for different types

For example:

```haskell
main = print (1+1) -- using integer operations

main = print (1.0 + 1.0) -- using floating point operations
-- this is called ad-hoc polymorphism or 'overloading'
```

# Type Classes I

How do we describe a collection of operations which certain types can implement? But each type might implement in it’s own way, a collection of operations, like that is called a **type class**

A type class in `haskell` is much more like an in interface imperative and object-oriented languages, it is not like a class

### Defining Silly Type Classes

```haskell
class Descriptive a where
	describe :: a -> String
-- 'a' is a type variable, and type 'a' is descriptive, if it has an implentation
-- 'describe', which takes an 'a' and returns a string

instance Descriptive Bool where
	describe True = "Yep!"
	describe False = "Don't bet on it"

main = print(describe True)
-- prints "Yep!"

instance Descriptive Int where
	describe 1 = "The loniest number"
	describe _ = "Just another number"

main = print(describe(1::Int))
-- prints "The loniest number"
-- Why do we have to write '1' as '1::Int'?
```

The reason we have to **explicitly** specify `Int` is that numerical constants themselves are overloaded in `haskell`. So `haskell` wouldn’t be able to infer that we want the implementation for the type `Int` without the annotation

### Writing functions that only work for types which are ‘Descriptive’

```haskell
descrLen :: a -> Int
descrLen x = length(describe x)
-- for x to be descriptive, 'a' must be an instance of descriptive, how do we do this?
-- we do this by putting a constraint on type 'a', as follows:
descrLen :: Descriptive a => a -> Int
-- this line is read as: 
-- descrLen has the following type, for all 'a', if 'a' is descriptive, then description
-- length has type 'a' to it

main = print(descrLen True) -- returns 4 as "Yep!" has 4 characters
main = print(descrLen (1::Int)) -- returns 18 as len("The loniest number") = 18
```

### Instance of descriptive for a type of lists

```haskell
instance Descriptive [a] where
	describe [] = "nothing else!"
	describe (x:xs) = ++ ",then" ++ describe(xs)
-- the problem here is we do not know how to describe 'x', because 'a' might not have 
-- an instance of descriptive. So we can do what we did above

instance (Descriptive a) => Descriptive [a] where
	describe [] = "nothing else!"
	describe (x:xs) = (describe x) ++ ",then" ++ describe(xs)
main = print(descrLen [True, True, False]) -- prints what True and False are assigned to
```

# Type Classes II

> Built-in class `Eq`
> 

The elements of the `Eq` type class are types whose elements can be compared for equality.

### Defining our own version of `Eq`

<aside>
🙀 If a function name is made of symbols, then by default it is treated as an infix operator (what is an infix operator?)
However if we want to treat it as a normal function, we have to wrap it around in brackets

</aside>

```haskell
class MyEq a where
	(===) :: a -> a -> Bool
	(=/=) :: a -> a -> Bool
	x =/= y = not(x === y)

instance MyEq Bool where
	False === False = True
	True === True = True
	_    === _    = False
main = print(False =/= True)
```

# Functions which require Several Class Constraints

We can define functions which specify that they are given types must be instances of more than one type class

```haskell
ifEqD :: (Descriptive a, MyEq a) => a -> a -> a -> a -> String
ifEqD :: w x y z = if (w === x) then (describe y) else (describe z)
main = print(ifEqD True True True False) -- prints "Yep!"

-- we only compare the first two arguments for equality and last two for output type,
-- we can change the names of the type variables

ifEqD :: (Descriptive b, Descriptive c,  MyEq a) => a -> a -> b -> c -> String
ifEqD :: w x y z = if (w === x) then (describe y) else (describe z)
main = print(ifEqD True True True False)
```

> Why are we doing this? 😕
> 

# Built-in Type Classes

There is a class `Show a` where the instances have to implement a function called `Show` which goes from `a` to `String`

```haskell
--- class Show a where
--- show :: a -> String
--------------------------------------------
main = print(length(show 17)) -- outputs 2 as length(17) = 2
main = print(take 5 ([1,2,3,4,5,6])) -- outputs [1,2,3,4,5]
main = print(take 5 (show [1,2,3,4,5,6])) -- outputs "[1,2," as this is String!!! IMP!!!
```

### Algebraic Data Types

```haskell
data Pcolor = R | G | B
main = print(R) -- gives us an error, as it does not know how to show R

data Pcolor = R | G | B deriving (Show)
main = print(R) -- prints R
```

For certain type classes, and certain types, including user-defined types, there is a default implementation of the built-in type class, in this case `Show` and we can ask for the type to be made an instance of the type class using the default implementation by using the `deriving` keyword

### Defining our own `Show` Implementation

```haskell
data Pcolor = R | G | B
instance Show Pcolor where
	show R = "red"
	show G = "green"
	show B = "blue"
main = print([R,G,B]) -- prints [red,green,blue]
```

# A custom Instance of `Eq`

```haskell
-- Similarly we mentioned the built-in typeclass Eq which provides == and /=
 
data ToyType =  Bird | Comet | Cloud deriving (Eq, Show)

data Mobile = Toy ToyType | Stick Mobile Mobile deriving (Show)
 
-- For Mobile we will define our own instance of Eq to get custom behaviour
 
instance Eq Mobile where
   Toy a == Toy b = a == b
   Stick w x == Stick y z = (w == y && x == z) || (w == z) && (x == y)
   _ == _ = False
 
main = print((Stick (Toy Bird) (Stick (Toy Cloud) (Toy Comet))) ==
       (Stick  (Stick (Toy Comet) (Toy Cloud) ) (Toy Bird))  ) -- should return True
```

We have a way of defining a collection of operations, which are meaningful together and which certain types might implement or might not. And as a side effect that gives us ad-hoc polymorphism, which is the idea that there are operations which are in some sense meaningfully the same operation, so they have the same name. But which each type might implement in it’s own way

# Non-Examinable Content

```haskell
-- we can add something like this at the start of the code:
{-# LANGUAGE RankNTypes #-}
-- this helps including a particular library (or something like that cba)
-- General form:
{-# LANGUAGE #-}
```