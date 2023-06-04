What is linear Programming.

Let's suppose we have a farmer who can plant wheat or barley or both, and he was a field of $A m^2$ in area, we have $x_B$ for the amount of Barley planted and $x_W$ for the amount of Wheat.

He also knows he will get $s_B$ for each metre of Barley and $s_W$ for each metre of wheat. (sale price)

He also knows he's gonna have to use $f_B$ and $f_W$ for the cost of fertiliser per metre. Same is true for pesticide $p_B,\ p_W$. Finally we know in this barn that he has $F$ amount of fertiliser and $P$ amount of pesticide.

So now we can make some constraints:
We want to maximise: $s_Bx_b + s_Wx_W$ 
We know that: $x_B + x_W \leq A$
and $x_Bf_B + x_Wf_W \leq F$
and $x_Bp_B + x_Wp_W \leq P$

This is the basic form of a linear programming problem - very much an optimisation problem.
- We want to minimise or maximise something given a set of constraints.
- Where all constraints are expressed as linear relationships, and they are all inequalities.
- All variables must be greater than or equal to zero.

### Visualising the problem...
it's only ever easy to visualise when there are 2 variables which fortunately is the case in the above example.

And one additional constraint is that: $x_B \geq 0$ and $x_W  \geq 0$

Let's put some numbers:
$A = 4,\ P = 30,\ F_b = 2,\ F_w = 10,\ P_b = 10,$ $P_b = 2, S_b = 5,\ S_w = 4$

Now replace all the constraints with the new constants: 

![[Pasted image 20230227124854.png|300]]

Visualise this on a graph now!

> ![[Pasted image 20230227124959.png|400]]
> ![[Pasted image 20230227125044.png|400]]
> ![[Pasted image 20230227125143.png|400]]
> ![[Pasted image 20230227125224.png|400]]
> Now we have found the feasible region, the white region, so now we need to find the best solution that falls in this space. (hint to do so, look at the constraint we have to maximise)
> ![[Pasted image 20230227125339.png|400]]
> Note the red line here, this is the gradient of the line we must maximise, now we can use the gradient to move the equation up and down to attempt to maximise the output:
> ![[Pasted image 20230227125806.png|400]]
> See the value has now increased but we can increase it more.
> ![[Pasted image 20230227125855.png|400]]
> Note here, this is the highest we can move our line such that it has a point within the feasible region whilst being the maximum value - also note that the solution also occurs at a vertex (where 2 line intersect) and this is always the case

All the constraints in the form of $... \leq number$ is referred to as the standard form and the thing we are maximising is referred to as the objective function.

We can represent these problems with Matrices and Vectors

![[Pasted image 20230227194525.png|400]]

![[Pasted image 20230227194624.png|400]]

We know what $x$, it's the variables.

![[Pasted image 20230227194717.png|200]]

What is $c$? Well it's multiplied by $x$ and it's the maximise row so:

![[Pasted image 20230227194751.png|200]]

Now $b$, it's everything that is less than or equal to $b$: 

![[Pasted image 20230227200941.png]]

![[Pasted image 20230227200952.png]]


goto: [[COMP26120|Home COMP26120]]
