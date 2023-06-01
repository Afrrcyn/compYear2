[[COMP27112]]


>[!info] A blob
>Is a set of pixels that:
>- Share some property - could be intensity
>- Are connected

Are properties of two adjacent pixels sufficiently similar to infer that they're from the same object?

Are a pixels properties sufficiently similar to the properties of the adjacent blob for it to be included?

For example, for a binary image, values are either 0 or 1 and so we can say that pixels are from the same object if they have the same value (and are connected)

We first need to look at how pixels are connected?

![[Pasted image 20230418100512.png|500]]

Is it only up and down or will we take into consideration the diagonal pixels as well?

And both have their problems for 4-connected, objects joining at corners can be disconnected such as this:

![[Pasted image 20230418100630.png|300]]

And for 8-connected, it solves the corner problem but can pierce thin objects... (two lines intersecting can be seen as one object)

![[Pasted image 20230418100711.png|300]]

>[!info] Region Labelling
>Aims to:
>- Identify groups of contiguous pixels
>- Label separate blobs

So for example:

1. Perform some sort of thresholding.
2. Label each region
3. Now we know the number of items in the image.

But we can run into problems...

Overlapping objects can be counted as 2, or for a single object, the thresholding may cause it to be split into more than one object and so is counted as such...

These can be solved with *Dilation*, *Erosion*, *Opening* and *Closing*. Morphological operations.

Dilation Enlarges an object. (Opening)
Erosion erodes the object. (Closing)

>[!example] Algorithm for Connected Component Analysis
>First Pass
>- if zero neighbours have a label, pixel receives the next free label
>- if one or more neighbours have the same label, pixel receives the same label
>- if two or more labels have different labels, pixel receives one of them and the equivalence is recorded...
>
>Second Pass
>- Re-label all equivalent labels

What about Boundaries of blobs?
- Scan the image from left to right and top to bottom
- First object pixel found is the start point $O_0$ (save location of this so we know when to stop!)
- Pixel to the left (must be background) is $B_0$

![[Pasted image 20230418192418.png|400]]

And we go around clockwise, looking at neighbours... so we know we're done because when we reach $O_0$ - if the next pixel after that is $O_1$, then we know we're done!

![[Pasted image 20230526164403.png|400]]

### Blob Descriptions!

##### Moments from Physics:

![[Pasted image 20230418192925.png|600]]![[Pasted image 20230418193016.png|600]]![[Pasted image 20230418193104.png|600]]![[Pasted image 20230418193303.png|600]]![[Pasted image 20230418193352.png|600]] ^f78aa1

So why did we look at all of this? Well in an image we want to see where the centre of gravity of a blob. Assuming that each pixel has equal weight, we can do:

![[Pasted image 20230418193838.png|500]]

So we work out the average $x$, using this [[#^f78aa1|equation]] (last one!)

![[Pasted image 20230418194104.png|600]]

And this is the formal definition of the above calculation:

![[Pasted image 20230418194328.png|500]]

This is central moments of area:

![[Pasted image 20230418194625.png|500]]

So the above makes all points relative to the centre as opposed to relative to the origin of the image, allows for the blob to be relocatable.

- Moments can be defined for all values of $\alpha$ and $\beta$
- There's a limit to the number of useful ones
- Can use lower order moments to make higher order ones invariant to:
	- Position
	- Orientation
	- Size of region
- Can compute for non-binary images
- Can modify computation to use labelled blobs
- Values of the moments can be used to discriminate blobs based on size and shape.

##### Chain Codes

![[Pasted image 20230418201225.png|600]]

![[Pasted image 20230418201243.png|600]]

We can use chain codes to describe a path around a blob boundary, chain code is cyclic, we could start anywhere in the code and get to the end and continue from the start!

Note however that this is not *rotationally invariant*, if the shape is rotated 90 degrees a different code will be produced.

Chain codes also allow for a more compact representation:

![[Pasted image 20230418201613.png|500]]

This allows you to compare blobs but also to re-create the blob.

To allow the chain code to be rotationally invariant we can instead do differential chain codes:

![[Pasted image 20230418201959.png|500]]

This is hard-work to draw this out.. so there has to be an easier way..

![[Pasted image 20230418202152.png|500]]

This allows for far easier comparison of chain codes!

We can also take the perimeter from an ordinary chain code - noting the fact that we cannot take the perimeter for differential chain codes!

![[Pasted image 20230419095208.png|600]]

How do we find the area? Let's look at some of the algorithms:

![[Pasted image 20230419095402.png|600]]

Note that this calculates the area and not the pixel count! However the two should be quite close. How can we get the coordinates from the chain code?

![[Pasted image 20230419100200.png|500]]

How can we track blobs over a time period? We need to look for invariant properties possibly? Will these be the same over the two time periods? We may have many blobs which we need to track *or* the blob population may change.

- $N$ blobs in frame $t$
- $M$ blobs in frame $t+1$
- Which subset of $N$ best match the subset of $M$
- A LARGE number of combinations...

So for each blob, maintain:
- Current Location
- Current Velocity (How far and in what direction did it move?)
- Invariants

We can then predict the location of the blob in the next frame.

And finally, we can verify and check if the blob is near the predicted location, Do predicted blob and this blob have the same invariants...

We may also check if the blob is near the position,,
- Velocity estimate may be incorrect
- Velocity may have even changed
- We should also maintain an estimate of error in Velocity

Some invariants may even change with
- Lighting 
- Orientation
So we have to check if invariants are roughly the same,,,


What if there are multiple blobs at the predicted location? We need to record this - incase they depart in future..

What if there is no blob in predicted location and the radius? Well try to keep track of the blob for a couple frames to see if you can keep track of it.

And blobs with no prediction means there's a new blob