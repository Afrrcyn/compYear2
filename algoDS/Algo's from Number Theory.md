## El Gamal Encryption

A one way function is easy to compute but hard to compute.. formally a function $f$ is one-way if it can be computed in polynomial time but any polynomial time randomised algorithm $F$ that attempts to compute a pseudo-inverse for $f$ succeeds with negligible probability.

This makes it hard in the *average case*

El gamal encryption is based on the observation that it's easy to compute $y$ in $$y=a^b\mod p$$But even knowing $y,\ a$ and $p$, it is average case hard to find $b$

El Gamal creates public keys by finding primitive roots module n of a prime. (You only need to know what a primitive root is! And you should be able to check if 2 numbers are relatively prime (use Euclids Algo for this)) 

To encrypt a message using El Gamal Encryption, you need to find a random $k$ which is relatively prime to $p - 1$ (where $p$ is a prime appearing in the public key) and then compute $a^b \mod k$ for various numbers. In order to do this you will need to use a fast modular exponentiation algo.

Inn order to decrypt a message you need to be able to find the inverse modulo some prime of a number.  You can do this using Euclids Extended Algo

[All of the above will be covered below, it's just a summary!]

### Euclids Algo

- The greatest common divisor or highest common factor of two numbers is the largest integer that divides both of them.
- If the highest common factor of two numbers is 1, then we can say that they are ==relatively prime==
- Note that `gcd(a, b) = gcd(b, a mod b)`

So how do we calculate it? Using this fact: `gcd(a, b) = gcd(b, a mod b)`, we can then calculate the answer.

```python
def gcd(a, b):
	if b == 0:
		return a
	else:
		r = a % b # a mod b
		return gcd(b, r)
```


### Modular Arithmetic and Primitive Roots
$$\mathbb{Z}_n = \{0,1,...,n-1\}$$
- We can add and multiply things in $\mathbb{Z}_n$ using modulo $n$ And usefully:
	- $(a+b) \mod n \equiv (a \mod n + b \mod n) \mod n$ 
	- $ab \mod n \equiv (a \mod n)(b \mod n) \mod n$
- But multiplicative inverses are also integers in $\mathbb{Z}_n$ not fractions...
	- $5 \times 2 \mod 9 \equiv 1$
	- (5 is the multiplicative inverse of 2 for $n \mod 9$ )
- However multiplicative inverses don't always exists...

If we use primes... $$p \text{ is prime, } \mathbb{Z}_p = \{0,1,..., p-1\}$$
- `gcd(x, n) = 1` iff `x` has a multiplicative inverse in $\mathbb{Z}_n$ 
- So multiplicative inverses exist for all numbers except 0 module prime numbers.

> [!info] New Notation...
> $\mathbb{Z}_n ^*$ is the set of numbers less than $n$ that have multiplicative inverses module $n$
> - Note that this means $\mathbb{Z}_n ^* = \{1,2,...,p-1\}$ for primes

![[Pasted image 20230314201554.png|400]]

What happens if we look at the exponents of $3 \mod 7$..

$3^1 = 3,\ 3^2 = 2,\ 3^3 = 6, 3^4 = 4,\ 3^5 = 5, 3^6 = 1$ 

- thus the order of 3 is $6$ *or* ($7 - 1$) and we have all the elements of $\mathbb{Z}_7 ^*$
- We call 3 a generator of $\mathbb{Z}_7 ^*$ or a primitive root modulo $7$ ***and*** if $p$ is prime then a generator exists.

In El gamal Encryption someone picks a prime, $p$ and a primitive root $g$
The recipient of a message picks a private key $x \in \mathbb{Z}_p ^*$, The public key is: $$(p, g, (g^x \text{ mod } p))$$
### Fast Modular Exponentiation and Discrete Logarithm
https://www.youtube.com/watch?v=8r4-5k-o1QE

![[Pasted image 20230314202855.png|700]]

The two textbooks have two different algorithms for this algorithm. (One uses recursion and the other iteration)

These algorithms would calculate $7^{11} \text{ mod } 11$ for example...

Discrete Logarithm

The discrete logarithm of an integer $y$ to the base $b$ is an integer $x$ such that $$b^x\equiv y \text{ mod } n$$There is no fast algorithm for computing this..

(this is the inverse of the exponentiation.. so the modular exponentiation is a good one way function)

If we have someones public key $(p,\ g,\ y)$ and we want to encrypt a number $M$ then we pick a random integer $k$ that is relatively prime to $p -1$ and compute: $$\begin{align} a \leftarrow g^k \text{ mod } p \\ b \leftarrow My^k \text{ mod } p \end{align}$$ And send $(a,\ b)$

### Multiplicative Inverses

![[Pasted image 20230315090106.png|400]]

Extended Euclids
```python
def ExtendedEuclid(a, b):
	if b == 0:
		return (a, 1, 0)
	q = a % b
	# let r be such that a = rb + q
	r = (a - q) / b
	(d, i, j) = ExtendedEuclid(b, a % b)
	return (d, j, i - jr)
```

![[Pasted image 20230315093109.png|500]]


goto: [[COMP26120|Home COMP26120]]



