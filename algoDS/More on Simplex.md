### What happens when the origin is not a solution?

![[Pasted image 20230313191451.png|300]]

The problem here is $2x_1 + x_2 \geq 12$, this does not allow the origin to be a solution and we can see this here with a graph:

![[Pasted image 20230313191559.png|500]]

So what happens if we convert to slack form and attempt to solve it with the simplex algorithm?

![[Pasted image 20230313191707.png|400]]

And our tableaux looks like this:

![[Pasted image 20230313191746.png|400]]

See the all the right hand side numbers (our basic variables?) need to be non-negative and this here is not the case.. This is because our basic solution will give $s_2 = -12$..

![[Pasted image 20230313192418.png|500]]

So let's change it now:

![[Pasted image 20230313192659.png|400]]

$M_1$ is supposed to be massive such that if $a_1$ is at all positive, that term will be so big it will drown out $x_1$ and $x_2$. Any solution in which $a_1 = 0$ will not be the optimal solution.

![[Pasted image 20230313192931.png|500]]

Basic Solution: $s_1 = 10,\ s_2 = -12$, so how did this at all help??

So let's pivot column $a_1$ with the row that has a minus in the final column:

![[Pasted image 20230313193243.png|400]]

We can leave the first row, negate the second row and the third row we simply add (row2 * $M_1$), this makes the values into linear equations involving $M_1$...

Now we just continue to apply the standard simplex method. Remember that we pick the smallest number (so negative)

![[Pasted image 20230313194124.png|500]]

So we pick the next smallest one, and do everything that is expected of us trying to get all the values in the column to zero bar the correct row.

Remember when calculating which variable is going to be entering and which one is going to be exiting that we don't like negative slack and upon seeing negative slack we do not use that variable as an exiting variable. (see the below example)

![[Pasted image 20230313194449.png|500]]

$x_1 = 10,\ x_2 = 0$

### What happens when the solution space is unbounded?

here's an example:

![[Pasted image 20230313194751.png|300]]

And what does this look like as a graph?

![[Pasted image 20230313194818.png|400]]

See here $x_2$ can get bigger and bigger, and the objective function can get bigger and bigger..

So let's just start with the simplex method:

![[Pasted image 20230313200036.png|400]]

So $x_1$ is clearly our entering variable and our exiting variable is $s_1$ (top row) and perform the appropriate operations:

![[Pasted image 20230313200254.png|500]]

(note bottom right is a $20$) So we're pivoting $x_2$ now, and we'll exit the $s_2$ variable. And perform the correct operations:

![[Pasted image 20230313200742.png|400]]

WE can do a pivot on $s_1$ but now we have negative slack for both our options and using any of those would take us into an infeasible region... So there's no pivot we can make that takes us towards a vertex within the feasible region.. and so we have an unbounded problem and you can see this when we have a column filled with all negatives:

![[Pasted image 20230313201016.png|400]]

### Zero Slack / Degeneracy

Let's look at an example:

![[Pasted image 20230313201503.png|300]]

So we have not included an objective function because we will look at the situation in which $x_1$ is our entering variable and when $x_2$ is our entering variable. So no need for the objective function as we would have to change it.

![[Pasted image 20230313201646.png|500]]

So let's look at what happens when we pivot... (Remember that we actually have 3 lines intersecting at the origin... 2  axis and one of the constraint lines)

![[Pasted image 20230313202328.png|500]]

Let's say we pick $x_1$ as our entering variable. Picking $s_1$ as the exiting variable, this will make a value in the final column negative and we do not want this! If we pick $s_2$ that still gives us a value at the origin...

So staying where you are in regards to which vertex we moved to on the graph - this gives us the degeneracy. 

Oke, so what happens if we pick $x_2$ as your entering variable:
So our entering is $x_2$ and our exiting is $s_2$ and we stay at the origin... and if we exited on $s_1$ it would have moved us into a better feasible vertex - even though it had the larger slack! So this is confusing our general rule which we normally use does not seem to apply here...

And what we actually have to do is look at the coefficients - for $x_2$ we have both a positive and negative coefficient:

![[Pasted image 20230313220345.png|400]]

Whereas when we were looking $x_1$, both were positive. As $x_1$ gets bigger and bigger, it will go outside the feasible region whereas with $x_2$ as it gets bigger and bigger it will stay within the feasible region. (when you have 0 slack and negative coefficient, then you can pick the smallest positive slack as your leaving variable) What do you do when you have multiple zero slacks? Always look for the smallest positive slack. What if the coefficients are positive with zero slack? Well then pick the smallest positive coefficient.

goto: [[COMP26120|Home COMP26120]]
