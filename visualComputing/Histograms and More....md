[[COMP27112]]

Imagine we have this greyscale image:

![[Pasted image 20230313090135.png|500]]

The histogram is simply a count of all the greyscale values in an image, so we have 3 $150$ grey values and if we count all the values:

![[Pasted image 20230313090235.png|500]]
[Although note you would start from 0 to 255, to get all grey values not just those within in the image]

Here is some pseudocode to do this:
```cpp
int histogram[256] = {0};

for (int i = 0; i < pixels in img; i++) {
	histogram[ img[i] ]++;
}
```

![[Pasted image 20230313093215.png|500]]

This is the histogram:

![[Pasted image 20230313093259.png|500]]

Okay let's look at one more:

![[Pasted image 20230313093435.png|500]]

What's up with all the gaps? This image looks perfectly normal to us, however the shading has been sampled and only has 64 shades of grey.

![[Pasted image 20230313093557.png|500]]
32 shades, we can start to see the patches and it does not look as natural.

##### Brightness Adjustment:
![[Pasted image 20230313093721.png|500]]![[Pasted image 20230313093927.png|500]]
note that we've not actually gained anymore information but rather lost information - all the lighter greys have been changed to white so we lost some shades of grey..

##### Contrast Adjustment

![[Pasted image 20230313094044.png|500]]

This time instead of adding a constant we have multiplied by a constant.

![[Pasted image 20230313094131.png|500]]

This has produces some gaps, we're now missing grey levels and this makes sense if you image $k = 2$ for grey = 1, this goes to 2, 2 goes to 4, 4 goes to 9 so we end up with gaps in our data, so be wary of the constant that you multiply by...

##### Clipping
What is $255 + 1$ when dealing with unsigned bytes? 
$= 0$
As it causes an overflow!

So what happens if you add 50 to every pixel to increase it's brightness?

![[Pasted image 20230313094527.png|500]]

When you don't have clipping you allow values to overflow, allowing whites to go to blacks which does not look natural when you're trying to increase the brightness. So we introduce clipping.

```cpp
uint8 px = 250
...
int32 p = px;
p += 50;
if (p > 255) p = 255;
if (p < 0) p = 0;
// above code ensures we can safely put p back into a byte
px = p
```
So by storing it in 32 bits, it allows us to capture the overflow without losing the information so we can catch clipping!

##### Input-Output Mapping

![[Pasted image 20230313094930.png|500]]![[Pasted image 20230313095015.png|500]]



When we look at showing these relations on graphs:

![[Pasted image 20230313095120.png|500]]

Low gradient changes the contrast by a small amount and a high gradient changes it by a large amount.

Mappings can be created in code with a Mathematical formula. 
- `img[r][c] = img[r][c] * 1.5`
or with a lookup table
- `map[256] = [0, 2, 3, 5, 6, 8, 9, ...]`
- `img[r][c] = map[ img[r][c] ]`

The lookup table is constant time (and thus faster to use!) but uses more memory..

##### Solarise

![[Pasted image 20230313110013.png|500]]

essentially if we're greater than some threshold, just take the negative, otherwise leave as it is.

And the mapping looks like this

![[Pasted image 20230313110127.png|500]]

##### Min-Max Linear Stretch

![[Pasted image 20230313110211.png|500]]

This gives more variation within the greys, if it's dark grey send it to black `0 if g < min` if we have some light greys send it to white `255 if g > max`, otherwise we just have some slope:

![[Pasted image 20230313110412.png|500]]

##### Thresholding

![[Pasted image 20230313110457.png|500]]

Turns an image into monochrome and our mapping looks like:

![[Pasted image 20230313110526.png|500]]

But how do we choose the value for the threshold $T$?

We have manual selection:

![[Pasted image 20230313110602.png|500]]

But manual selection of $T$ requires the creation of a histogram and for a person to examine that imagine, can this be done automatically?

Percentile calculation of $T$
- if we know that an object should occupy a certain percentage of pixels in an image we can automatically select $T$
- Example: a biscuit on a conveyer line should occupy about $41\%$ of the image

Algorithm:
- Calculate the number of pixel that should be object (% of image size)
- Create image histogram
- Accumulate frequencies until total exceeds number of expected object pixels
- Return current grey level as $T$

![[Pasted image 20230313111709.png|500]]

This algorithm is not actually affected by lighting! Due to the fact we look at the first 41% of greyscale values.

Finding a threshold between the means of lower and upper parts of the histogram:

![[Pasted image 20230313112021.png|500]]

Pseudocode for this:

![[Pasted image 20230313112133.png|600]]

Otsu's Method:

![[Pasted image 20230313112152.png|600]]

![[Pasted image 20230313112206.png|600]]

There's a lot of complicated mathematics in Computer Vision but spending some time looking through the algorithm will show you that it's a lot simpler than it looks.

![[Pasted image 20230313112315.png|500]]

![[Pasted image 20230313112430.png|500]]

Both must choose a similar value... so the p-tile is quite accurate as seen with Otsu's method

![[Pasted image 20230313112545.png|500]]

If you see here picking that $T$, we misclassify some of the object, and this always happens - there's nothing we can do about it.

![[Pasted image 20230313112639.png|500]]

I think a lot of this depends on how you greyscale the image, as some of the object will naturally have the same shade as the background and so maybe changing up your greyscale can fix this?

### Geometric Transformations

Scaling:

![[Pasted image 20230313113247.png|500]]

So we have our X, Y coordinates and multiply by the Scaling Matrix - we have seen this in the first part of this course. Keeping the output image the same size as the input image means we need to interpolate pixels...

Translations:

![[Pasted image 20230313113426.png|500]]

And you can do both at the same time!

![[Pasted image 20230313113457.png|500]]

How can we do this in a single matrix multiplication? Remember from the first part? Augmented Matrices..

![[Pasted image 20230313113605.png|500]]

![[Pasted image 20230313114040.png|500]]

`warpAffine()`(takes `2x3` matrix) and `warpPerspective()`(takes `3x3` matrix) can be used in openCV

Reflections and Flipping:

![[Pasted image 20230313114135.png|500]]

But do remember that the origin is on the top-left corner, so that's why we have to put the $480$ in the above matrix so that the image is not flipped outside of the view.


Shear:

![[Pasted image 20230313114318.png|500]]

Rotation:

![[Pasted image 20230313114347.png|500]]

Note that we have seen most of these in the initial part of the course!

![[Pasted image 20230313114449.png|500]]
Note how the bottom entries do not serve a purpose, because they are just there for homogenous coordinates

All camera lens have some sort of barrel distortion:

![[Pasted image 20230313114607.png|500]]

The wider the angle of the camera the higher the amount of barrel distortion. There's also pincushion distortion:

![[Pasted image 20230313114709.png|500]]

![[Pasted image 20230313114841.png|500]]

As your move further from the radius the level of distortion increases and we can model it with the constants $k_n$ 

There's also Tangential Distortion. Using this equations we can try to model the distortion to reverse engineer to get the original image with no distortion.

![[Pasted image 20230313115039.png|500]]

Wide-Angle Lens

![[Pasted image 20230313115556.png|500]]

You can see the barrel distortion with the buildings curving...

Fisheye Lens

![[Pasted image 20230313115626.png|500]]

You can see the barrel and pincushioning distortion. Also on the corners (the trees) the colours have been changed, this is called ==chromatic abberation==

An image of a chessboard can be used by openCV to allow you to correct for camera distortion:

![[Pasted image 20230313115827.png|500]]

Note that you need several images and not just 1-3 images...

Interpolation: 

![[Pasted image 20230313123506.png|500]]


![[Pasted image 20230313123705.png|500]]

The shape is the same but it's too blocky and jagged..

![[Pasted image 20230313124218.png|500]]

okay so what about rotation, source pixels might no land in a destination pixel and thus we will have to round and the problem with that is that we may create holes in the destination pixels...

Solution:

Rather than cycling through source pixels and calculate the destination pixel, we work backwards and cycle through the destination pixels and calculate the source pixels.

![[Pasted image 20230313125559.png|500]]

This will require some interpolation as well... so closest neighbours.

