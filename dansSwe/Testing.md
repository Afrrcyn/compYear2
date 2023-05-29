#### Unit Testing
Unit testing is a software testing technique where individual units or components of a program are tested to ensure that they function correctly in isolation.

What makes a good unit test?
- That only tests one thing and do not build on other things.
And so when test a single component we will have to make use of stubs and mocks and ... to allow us to only test that part we care about.


- Dummy - A dummy is passed around but never used 
- Fake - A fake generally works as expected, but has some shortcut unsuitable for full production 
- Stub - A stub provides a canned answer to a particular invocation 
- Mock - A mock has pre-programmed expectations of how it will be called, and what will happen internally when it is called. They return different results in different contexts

So how can we use a Mock to Stub a method?
We use the `@Mock` for the entity and `@MockBean` for the class that contains the functions we care about. And then we do:
```java
when(<cond>).then(do something ...);
when(<cond>).thenReturn(entity?);
when(<cond>).thenThrow(new Exception());
```
This is a stub.


We can also verify if something was called or even how many times it was called:
```java
verify(venueService, times(1)).delete(1L);
verify(venueService, atMost(3)).delete(1L);
verify(venueService, never()).delete(1L);
```


Validation - does it follow the requirements
Verification - is it bug-free

A defect is a missing requirement, function, performance, usability issues and a failure is the manifestation of that defect - whether it fails or does not fail

Static Analysis - code is not ran - code review, IDE, it can be both manual or automatic.
Dynamic Analysis - done by a test-suite usually and can be done either manually or automatically.

White Box testing - We can test the internal functions and structures and we see how and what it's doing. (Good for the dev)
Black Box testing - we have no knowledge of the internals, we simply looking at the inputs and outputs. (Good for thinking about the user)

We know what Unit and Integration tests are, but there are also something called System tests, which test the end-to-end points, such as:

Logging in > Make a Post > Log out

There are many types of System Tests:
- Soak
- Spike/Stress
- Load
- Performance
- Scalability
- Security
- GUI
- Usability
- Accessibility

Acceptance tests are test for the user to ensure their ends work and their requirements are met.

Regression tests are used when maintenance and refactoring occurs, Regression tests basically consists of the entire test-suite.

![[Pasted image 20230528114652.png]]


goto: [[Flaky **Tests]]**, [[SWE 2 Home Page]]