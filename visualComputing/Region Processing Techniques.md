[[COMP27112]]

These are functions that involve more than one source pixel:

Example:
$$I_H(x, y) = |I(x, y) - I(x + 1, y)|$$

(this function here will take the vertical edges)

And we can create a similar function to find the horizontal edges: $$I_V(x, y) = |I(x, y) - I(x, y+1)|$$
So we've created two different images that find different edges, how can we combine these two images?

$$I'(x, y) = I_H(x, y) \wedge I_V(x, y)$$

There's another method:

![[Pasted image 20230320194321.png|500]]

But the most obvious way is to use the Euclidean Distance..

![[Pasted image 20230320194541.png|500]]

![[Pasted image 20230320194823.png|400]]

Is there a better way to find the edges of an image? Yes:

##### Convolution

![[Pasted image 20230320200931.png|600]]

![[Pasted image 20230320201157.png|500]]

$W$ convolved with $I(x, y)$.. so in this case the kernel $W$ is a 3 by 3 matrix, so in our example, we consider $W_4$ to be the origin and consider $a$ and $b$ to be 1, so that's why there's a 3 by 3 matrix because we're going to be looking at all the surrounding pixels when $a$ and $b$ equal 1

![[Pasted image 20230320201639.png|500]]

There's actually 2 very similar operations, Convolution and Correlation, convolution flips the kernel diagonally but correlation does not, most image processing software will actually do correlation but call it convolution...

>[!info] OpenCV
>Apply a filter kernel to an image with `filter2D()`

Most of the time this diagonal flip makes no difference as kernels tend to be symmetrical.

![[Pasted image 20230320202141.png|500]]

In point processing we can calculate a new pixel value and write it 'in-place' that is, back into the same image however we cannot do this with convolution as it requires use of the values around it and thus we need the original values...

Kernels are normally square and odd-sized so we know where the centre is.. Now the question is what about pixels that go off the edge?

![[Pasted image 20230320202913.png|400]]

We can either set them to 0, or resize the image to knock off all the edge pixels...

![[Pasted image 20230321084459.png|500]]

So what happens if we place a kernel in a location in the image where all the values are 255? It would be 255 x 9, so what we have to do is normalise the kernel by dividing the final value by 9, or divide the kernel values by 1/9.

Please also note that convolution is associative.

![[Pasted image 20230321084725.png|300]]

If we need to convolve an image twice, we can just combine the two kernels into one kernel and apply it once. Note that we can also split kernels up:

![[Pasted image 20230321084937.png|500]]

The next question is why would we want to split it up? Well if you look at the 3x3 Matrix we can note that for each pixel it will perform 9 Multiply and Additions where as the other 2 kernels together will only perform 6 Multiply and Additions per pixel.

##### Separable Kernels
![[Pasted image 20230601162639.png|600]]


##### Application of Convolution

Smoothing... remember earlier when we talking about normalisation for a kernel and we said we could multiply all the values within a kernel by 1/9

![[Pasted image 20230321085429.png|400]]

This kernel is known as an averaging kernel or a box filter - this actually blurs the image:

![[Pasted image 20230321085524.png|400]]![[Pasted image 20230321085544.png|400]]
We can also use a kernel to sharpen an image:

![[Pasted image 20230321085656.png|400]]

(Note here we end up with (5 * value) - (4 * values), this is how we try to ensure our values stays within one byte)

We can also use it for edge detection, one example is Laplacian:

![[Pasted image 20230321085933.png|400]]

Some of these tend to amplify noise, so how can we reduce the noise?

Noise is the deviation of a value from it's expected value..

![[Pasted image 20230321090150.png|400]]

So we can look at noise in two different types of ways:
- Anything within the imaging system that causes a change
	- Electrical Interference
	- Optical Aberration
- Anything that causes a change
	- Atmospheric Disturbances

![[Pasted image 20230321090523.png|500]]

So how can we reduce all of this?

![[Pasted image 20230321091123.png|400]]
Image Registration: Trying to find features in an image in order to align different sets of images.


![[Pasted image 20230321091753.png|400]]

It works for gaussian noise but has not worked so good for salt and pepper noise and we can solve this using Adaptive Smoothing.

![[Pasted image 20230321091943.png|400]]

![[Pasted image 20230321193350.png|400]]
The corners here are still nice and sharp however the salt and pepper has not really been fixed.

![[Pasted image 20230321193529.png|400]]

##### Gaussian Smoothing
We use the gaussian smoothing because it removes ringing, as it uses weighted smoothing where the weights are derived from a normal distribution (gaussian).
(Less weighting is given to the pixels further away from the centre of the kernel)

![[Pasted image 20230321194256.png|400]]

The edges are still quite blurred...

Another filter we can use is a median filter.

![[Pasted image 20230321194346.png|400]]
So, sort the values out, take the median out and that is your value.
![[Pasted image 20230601163038.png|450]]

![[Pasted image 20230321194515.png|400]]

Retains both the sharp edges and works perfectly with salt and pepper noise!
So the median is one of the best filters... And tends to remove only noise.

