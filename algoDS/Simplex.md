![[Pasted image 20230306123738.png|400]]

Solve this using a graph!

![[Pasted image 20230306125227.png|400]]

By having the origin a a solution it gives our algorithm a place to start...

Let's firstly introduce slack variables:

![[Pasted image 20230306173433.png|400]]

$-x_1 + x_2 + s_1 = 11$ and $s_1$ varies such that the solution always equals 11, using this logic we get:

![[Pasted image 20230306173545.png|400]]

Notice we did not get a slack variable for the non-negativity constraint and the maximise constraint.
(The above is known as slack form)

If any variables appear in more than one equation, then we can set it to 0. ($x_1$ and $x_2$) thus $s_1 = 11,\ s_2 = 27,\ s_3 = 90$ will satisfy all equations.

(This is not optimal...) But this is *a* solution.

>[!info] Basic Terminology
>- The variables that appear once are called basic variables
>- The other variables are non-basic variables
>- The solution where all the non-basic variables are zero is a basic solution.


![[Pasted image 20230306174535.png|500]]

Thus this is our feasible region and so the optimal solution must lie at one of these vertices:

![[Pasted image 20230306174804.png|400]]

[Note the vertices circled on the graph]

Note also for each line, we know that one of the slack variables equals zero:

![[Pasted image 20230306175105.png|300]]

What we do with the simplex algorithm is we do something called a pivot - which swaps a basic variable with a non-basic variable.

![[Pasted image 20230306175329.png|300]]

When you swap, you set the new non-basic variable to 0, but you have to be careful with which variables you swap, swapping the wrong variables can take you out of the feasible region.

So we can use brute force to go around all the vertices to find which vertex is the best.

Pivoting:

So we have our slack form:

![[Pasted image 20230306192345.png|400]]

Firstly put all slack variables in all equations:

![[Pasted image 20230306192421.png|400]]
(And Maintain Order!)

![[Pasted image 20230306192455.png|400]]

Can the maximise function to the format above...

![[Pasted image 20230306192525.png|400]]

We can convert this to augmented matrix and use Gaussian Elimination to help us work through the problem.

![[Pasted image 20230306192731.png|500]]

So let's a pick a non-basic variable with the largest coefficient, and so which basic variable do we pivot with? So pick one with the least slack (least room to manoeuvre)

![[Pasted image 20230306193003.png|500]]

See here we have worked out the slack: 11/1, 27/1 and 90/5 and 11/1 has the least value thus we choose this to perform the pivot.

thus $x_2$ is the entering variable and $s_1$ is the leaving variable. And so we construct the new form of the augmented matrix such that we have a 1 in the $x_1$ column (and $s_1$ row) and the rest is 0 (in the $s_2$ and $s_3$ row)

So we use Gaussian Elimination:

![[Pasted image 20230306193549.png|500]]

Performing operations to ensure that the rest of the $x_2$ column is zero bar the first row. Now we have a new basic solution. And now 66 is now our value of the objective function.

What does this mean on the graph?

![[Pasted image 20230306193744.png|500]]

See here, we moved from the origin to the second circled point.

Anyway let's continue with this example:

![[Pasted image 20230306202228.png|500]]

We do not want negatives on the bottom row and thus we will choose to pivot $x_1$.  So we need to determine which variable has the least slack.. (ignoring negative slacks!!)

![[Pasted image 20230306202542.png|500]]

Notice we pick $s_3$ because $x_2$ is negative. Now we use Gaussian elimination, so remember to make the column $x_1$ row $s_3$ value $1$ and then the rest of the values in that column $0$

![[Pasted image 20230306203219.png|500]]

So we need to do another pivot: Entering variable is $s_1$ and the exiting variable: $s_2$

![[Pasted image 20230306203616.png|500]]

Use Gaussian Elimination.. (note we can use the old row for gaussian operations!)

![[Pasted image 20230306204315.png|500]]

So why do we stop the simplex method when we have no negative values in the very bottom row?

![[Pasted image 20230306205231.png|500]]

goto: [[COMP26120|Home COMP26120]]


