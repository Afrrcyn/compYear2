# Input and Output

# IO in Haskell

## Special type: IO

Values for `IO` are input/output behaviours

```haskell
main = (print "Hi!":: IO ()) -- :: IO() means of type IO with an empty tuple
```

A `haskell` program is just a complicated definition of an element of an IO type

We must define a constant main whose type is an IO type and all our `haskell` compiler is, is a system which calculates which input output behaviour this value is `print "Hi!" :: IO ()` and then writes an executable file which has that input output behaviour in particular

```haskell
a = print "a!"
b = print "b!"

main = (print "Hi!":: IO ())
-- output > Hi!
-- why does Haskell not output "a!" and "b!" ?
```

`Haskell` only outputs “Hi” because it only looks at the definition of `main`

`a` and `b` would only evaluate to a value describing an input output behaviour, but that input output behaviour does not happen unless it’s called `main`

`Haskell` just looks for an input output behaviour called `main` and makes an executable file which carries out that input output behaviour

```haskell
main = [print 1, print 2, print 3]
-- this would not compile, why?
```

This would not compile because `main` is supposed to be an `IO` type, but it’s a list in here, however if you take a `head` of that list, then that does compile and it just carries out the first of these actions 

```haskell
main = head [print 1, print 2, print 3]
-- outputs 1
```

Because IO values are ordinary values, we can return them from functions if we wanted

```haskell
green n = print ("Hi " ++ n)
main = greet Joe
-- outputs "Hi Joe"
```

In this case, `greet` is a function that takes a string and returns the description of an input output behaviour

### `getLine`

```haskell
main = getLine
```

You will have to type something in, in the terminal before the program terminates, basically an empty line, should be useful for taking an input

The type of `getLine` is `IO String`

```haskell
main = (getLine :: IO String)
```

### `return`

Another IO action called `return`

What it does is, takes an ordinary Haskell value and turns it into an IO value, and `return` does nothing - no actual input output behaviour but it results in the value it is returning to being available to future IO actions that might be carried out

```haskell
main = (return "Hi!":: IO String)
-- nothing is outputted
```

# IO Depending on a Value

Is there a function that returns function IO b to b? For example

```haskell
IO b -> b
-- no, there is no such function? why?
```

There is no such function because there is no way of turning an input output behaviour that results in a particular value into a mathematical value, because the value we get at the end of the input output behaviour depends on what the user does. Hence it is impossible for us to say which value is returned by the input output behaviour

What can be done then?

Use the **bind** operator `>>=`

Type of the bind operator:

```haskell
>>= :: IO b -> (b -> IO c) -> IO c

-- reading this code:
-- an 'IO b' is an input output behaviour that results in a value of b being
-- stored. Since we don't know what that value is, we cannot pass it to a 
-- function. But if we have a function that depends on b, we could then embed that
-- function into a more complicated IO behaviour, which takes the b that is returned
-- by this IO b, and plugs it into the function at the time when it's known.
-- Then, instead of trying to get the b out, we have to put the function that
-- manipulates the b into the input output behaviour that we are describing

-- summary: we write a function that takes a b, and returns IO c, and then the
-- result of all of it is going to be IO c
```

### Example of a function with an empty `IO ()`

```haskell
greet :: String -> IO()
-- what does the empty tuple 'IO()' mean?
-- a way of writing a type that has exactly one value, don't care about what the
-- value is. IO () does not return a value

greet n = print ("Hi " + n)
main = getLine >>= greet

-- output:
>> (blank line to give input, comes from getLine)
>> -- User inputs "Joe"
>> -- prints "Hi Joe"
```

```haskell
main = getLine >> greet
-- notice the difference, '>>' and '>>='
-- but that code would not work, why?
-- because 'greet' would need an argument now
main = getLine >> greet "Ali"
-- now, even if you enter into terminal `greet "Dan"`,
-- it will output:
>> "Hi Ali"
-- why?
-- because the value from `getLine` is now discarded
```

### Lambda Expressions

```haskell
main = getLine >>= (\n -> greet n)
>> -- enter "Ali"
>> "Hi Ali"
```

# Recursively defined IO

There are **no** control flow statements in `haskell`. `If then else` are also not control flow statements.

```haskell
main = putStrLn "Who is the greatest?" >> 
			 getLine >>=
			 \n -> if n == "Joe"
					then putStrLn "Don't forget it!"
						 else (return "Be serious!" >> 
									main)
-- this code would keep calling `main` until you enter "Joe"
-- once you enter "Joe" it will return saying "Don't forget it"
-- so recursively calling a function until a condition is met
>>> (input) Rizos
>>> "Be serious!"
>>> (input) Joe
>>> "Don't forget it!"
```

# `DO` Notation

Where would we use the `DO` notation in `Haskell`?

```haskell
main = putStrLn "Enter a name" >> 
				getLine >>=
				\n ->
				putStrLn "Enter another name:" >>
				getLine >>=
				\m ->
				putStrLn ("Hi " ++ n ++ " and " ++ m)
>>> (input) Joe
>>> (input) Pierre
>>> "Hi Joe and Pierre"
```

Instead, we can use the `DO` notation for:

Instead, we can use the `DO` notation for sequencing IO actions in a more readable way:

```haskell
main = do
				putStrLn "Enter a name"
				n <- getLine
				putStrLn "Enter another name:"
				m <- getLine
				putStrLn ("Hi " ++ n ++ " and " ++ m)
>>> (input) Joe
>>> (input) Pierre
>>> "Hi Joe and Pierre"
```

Note that we remove all binding operators, and the `n ← getLine` seems like setting n to a function, almost like using `Let`