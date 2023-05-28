Tests that cause failures without code changes are called flaky tests.

The most common way to reduce the impact of flaky tests is by re-running the tests. And we require a single pass execution to know the test passes.

>[!note] There are 13 categories of flakiness
>- Async Wait
>- Concurrency
>- Test Order Dependency 
>- Resource Leak 
>- Network 
>- Time 
>- IO
>- Randomness
>- Floating Point Operations
>- Unordered Collections
>- Too Restrictive Range 
>- Test Case Timeout
>- Platform Dependency
>- Test Suite Timeout
>
>These can be grouped into order-independent and non-order-independent.

A test $t$ can be order-dependent either:
- Another test $p$ running before $t$ disturbs its execution
- Another test $s$ is not run before $t$, so $t$ requires $s$ to run before it. 

1. In the first case, t is called a victim and $p$ is called a polluter. Test $p$ changes a shared state that $t$ tries to read from in a way that $t$ fails. When run in isolation, the victim, $t$ passes, as the state is not affected by the polluter. 
2. In the second case, $t$ is called a brittle and $s$ is called a state-setter. Test $t$ needs $s$ to set up a shared state

>[!note] 15th Category of Flakiness
>Not caused by the project itself but rather external components such as failing to install external



### Numbers
- Whilst flaky tests were equally as common in Python as Java the reasons in python were different; Order dependency caused 59%, 28% were test infrastructure and 13% attributed to use of network or API randomness.
- A 95% confidence that a passing test case is not flaky on average would require 170 reruns.
- For Non-order Dependent, we need to run 170 times for 95% confidence whereas for Order-Dependence we need only run 31 times !
- The problem with flaky tests is that it wastes time and resources - google spends between 2% and 16% of their resources re-running tests...
- The root cause of flakiness was 28% infrastructure, 59% Test-Order Dependency (with around 75% of those being ==victims==) and 13% Non-Order dependent (Mostly networks and API Randomness)


goto: [[SWE 2 Home Page]]