[[COMP27112]]

Hough transform helps us find lines in images

Say we have four points (0,0), (-10, 10), (2, 4) and (8, -8)
And we want a straight line to go through all 3 of these lines, how do we find that line?
We create a 2D array and use the columns for values of $m$ and rows for the values of $c$

![[Pasted image 20230425104301.png|500]]

Now find each cell that would go through that point! And increment it's value in the cell by 1, an empty cell just means 0.

![[Pasted image 20230425104455.png|500]]

![[Pasted image 20230425104609.png|500]]

![[Pasted image 20230425104959.png|500]]

![[Pasted image 20230425105031.png|500]]

Now we can see that there is 1 point that get's 3 points out of the 4 points - which is pretty good, m = -1 and c = 0. y = -mx.

for a vertical line $m = \infty$, so instead we can use polar coordinates.

![[Pasted image 20230425105429.png|500]]

where the new line is perpendicular to that of the line that cross all our points, so let's see how we get this. So use the normal method... For point (0,0)

![[Pasted image 20230425105832.png|500]]

but now our axis have changed!

![[Pasted image 20230425105916.png|500]]

![[Pasted image 20230425105945.png|500]]

![[Pasted image 20230425105931.png|500]]

So the highest number occurs at $\theta = 45 \degree$ and $r = 0$, so $\theta = -45 \degree$ because $\theta$ is the angle of the normal (I dunno what that means but yeah...)


![[Pasted image 20230425142722.png|500]]

### Binary Morphology
- Erosion
- Dilation
- Opening
- Closing

We use structuring elements and these can be different shapes!

![[Pasted image 20230425143546.png|500]]

These are passed over an image one pixel at a time.

##### In Erosion
If the centre of structuring element is over an object pixel and any of the other elements of the SE are over a background pixel, the pixel under the centre becomes background.
Note that the output must be drawn into a new image.

##### In Dilation
If the centre of the SE is over a background pixel and any of the other elements of the SE are over an object pixel , the pixel under the centre becomes object.
Noting that the output must be drawn onto a new image!

![[Pasted image 20230425145635.png|400]]

![[Pasted image 20230425145710.png|500]]


