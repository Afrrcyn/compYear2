[[COMP27112]]

An edge is an extended significant local change in image intensity.

![[Pasted image 20230321202915.png|500]]

![[Pasted image 20230321213121.png|500]]

Once we work out the gradient we need a way to combine the horizontal and vertical gradient - we've looked at this!

![[Pasted image 20230321213325.png|400]]

![[Pasted image 20230321213654.png|500]]

Here the edge is the dotted line, and so when we calculate the gradient, we're actually calculating the normal to the edge (non-dotted-line)

![[Pasted image 20230321213808.png|500]]

This is one of the first ever edge detectors, the problem with this is that there's no center so where do we put the edge in a 2x2 kernel?

Then:

![[Pasted image 20230321213946.png|500]]

Okay but they seem so similar:

![[Pasted image 20230321214054.png|500]]

It's to notice but Prewitt has less noise and thicker edges.

How do these 2 deal with noise?

![[Pasted image 20230321220755.png|400]]

Prewitt actually has less noise. And so why does the prewitt reduce noise? Well prewitt kernel separates into an averaging filter that smooths across the rows and edge detector along the horizontal lines.

There's another filter for edge detection called Sobel:

![[Pasted image 20230322085749.png|400]]

This is a weighted average filter that smooths along the rows and an edge detector which finds horizontal edges.

An Prewitt and Sobel are very very similar!

![[Pasted image 20230322090005.png|400]]

This is an actual edge detector that doesn't just find a gradient (like the ones above)

>[!info] Reminder
>![[Pasted image 20230322090127.png|450]]


Remember that a Gaussian filter does not produce ringing at discontinuities..
> Ringing refers to an artifact that can occur when an image is filtered using certain types of filters, such as sharp high-pass filters. Ringing appears as a series of oscillations or ripples around sharp edges or contrast changes in the image.

![[Pasted image 20230322090257.png|400]]

![[Pasted image 20230322090410.png|600]]

Doing this will help with our edge detection:

![[Pasted image 20230322090444.png|400]]

![[Pasted image 20230322090650.png|500]]![[Pasted image 20230322090807.png|500]]![[Pasted image 20230322090931.png|500]]![[Pasted image 20230322091109.png|500]]![[Pasted image 20230322091308.png|500]]

![[Pasted image 20230322091423.png|350]]

Canny sugget that $T_H$ should be two or three times $T_L$

![[Pasted image 20230322091600.png|600]]

![[Pasted image 20230322091707.png|600]]

### Laplacian

![[Pasted image 20230322100629.png|400]]

![[Pasted image 20230322100808.png|500]]

![[Pasted image 20230322101240.png|500]]

The gaussian blur helps to get rid of all the noise producing stronger lines

![[Pasted image 20230322101322.png|600]]

Then there's another operator:

![[Pasted image 20230322192058.png|500]]

This is still finding all the edges in the image, but it's not as good you can see some noise! If you ever download an image and notice there are bits of noise in 8x8 blocks, there is likely due to JPEG artefacts caused by too much compression..

Finally template matching?

![[Pasted image 20230322192648.png|450]]

![[Pasted image 20230322192745.png|450]]

![[Pasted image 20230322192915.png|450]]
![[Pasted image 20230322193109.png|450]]

Note that, if the template has a different perspective compared to the image, this can cause problems and much better results can be achieved with ==feature detection==.

