How easy is it to find Prime Numbers?

Let $\pi(n)$ be the prime distribution function specifying the number of primes that are less than or equal to $n$ $$\pi(10) = 4\ \ (2, 3, 5, 7)$$

The prime number theorem states that: $$\text{lim}_{n \rightarrow \infty} \ \ \frac{\pi(n)}{n / \ln n} = 1$$ And so $\frac{n}{\ln n}$ gives an approximation for $\pi(n)$

So, therefore if we pick a large number $n$ there is a $\frac{\pi(n)}{n}$ chance that it is a prime, so there's a $\frac{1}{\ln n}$ chance it's prime.

If we want a prime of the same size as $n$ (same number of bits!) then if we pick $\ln n$ numbers of that size then we will prolly find a prime. So for a 1024 bit number we will take approximately $\ln(2^{1024})$ or roughly $709$ attempts.

So the ease of finding a prime is more dependent on the ease of checking whether a random number is prime...

##### First Attempt (Trial Division)

![[Pasted image 20230417190550.png|500]]

So what's the complexity, at first glance: $O(\sqrt{x})$ and if $x$ is $n$ bits then this is $O(2^{\frac{n}{2}})$ which is actually not very good...

Okay so let's look at exploiting Fermat's Little Theorem...
- Recall that if $p$ is prime and $x$ is an integer where $(x \mod p) \neq 0$ then $x^{p-1} \equiv 1 (\mod p)$ 
- So we could use fast modular exponentiation (linear complexity)

1. Pick an $x$ at random and if $(x^{p-1} \mod p) \neq 1$ then $p$ is not prime!
2. But what if $x^{p-1} \equiv 1 (\mod p)$?
3. So maybe we pick multiple $x$?

Well, this strategy may not work (when step 2 is true) as there are Carmichael Numbers, these numbers have $x^{n-1} \equiv 1 (\mod n)$ for all $1 \leq x \leq n$ but $n$ is composite, and if we have Carmichael numbers, picking a new $x$ won't ever work.... (we'll get false primes otherwise..)

### Randomised Primality Testing
(Will be used with Fermat's Last Theorem)

Assume we have a function `witness(x, n)` that works as follows
- if $n$ is prime then `witness(x, n)` returns false
- if $n$ is composite then `witness(x, n)` returns false with probability $q < 1$ 

![[Pasted image 20230417193024.png|400]]

- The algorithm is supposed to return false (meaning $n$ is prime) with an error of probability $2^{-k}$
- $k$ is called the confidence parameter
- it will return false with probability $q^t$ (assuming our $x$'s are picked independently)
- This is a common pattern in randomised algorithms. We force the probability of error down below some confidence parameter by repeated applications with some random input.

![[Pasted image 20230418090037.png]]

##### Rabin-Miller Primality Test
Remember:

> Assume we have a function `witness(x, n)` that works as follows
> - if $n$ is prime then `witness(x, n)` returns false
> - if $n$ is composite then `witness(x, n)` returns false with probability $q < 1$ 
> 

So we could start with Fermat's Little Theorem and check that $x^{n-1} \mod n \equiv 1$ and live with the fact that there's a small chance that $n$ is a Carmichael number.

>[!info] Some facts before we start
>- if $p$ is a prime number then if $x^2 \mod p \equiv 1$ then either $x \equiv 1 (\mod p)$ or $x \equiv -1 (\mod p)$
>- Moreover, if $n$ is odd then $n-1 = 2^tm$ for some $m$
>- Recall that $(-1)^2 = 1$ and $1^2 = 1$


![[Pasted image 20230418091511.png|500]]

![[Pasted image 20230418091712.png|500]]

![[Pasted image 20230418092038.png|500]]


What is the error probability?
- If $n$ is composite then there are at most $\frac{n-1}{4}$ values for $x$ such that `witness(x, n)` returns false
- So $q = \frac{1}{4}$ which means that $t = \frac{k}{2}$, if we use this witness function in our randomised primality testing algorithm.
- This is called the Rabin-Miller algorithm

goto: [[COMP26120|Home COMP26120]]




