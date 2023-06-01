[[COMP27112]]
The Camera Analogy
1. Arrange the objects into the desired composition
2. Position the camera and point it at the scene.
3. Choose a camera lens (wide-angle, zoom, ...)
4. Decide the size of the final photograph

And there are parallels of this in graphics:

![[Pasted image 20230214143431.png|500]]

### 3D viewing pipeline

![[Pasted image 20230214143644.png|500]]

1. Positions object where you want it
2. Where do you set up the camera
3. Takes data and projects it into an image
4. Clip to a particular volume...

In OpenGL, Matrix $M$ and $V$ (look at the image above) are combined into a single matrix $\text{ModelView}$ called $C$.

In most graphics systems there is a default view:

![[Pasted image 20230214144016.png|500]]

![[Pasted image 20230214193200.png|500]]

Therefore the z = 0 plane then gets mapped to the display screen and whatever geometry is there gets scan converted (aka rasterised) and the z-buffer applied.

##### Model Transformation

![[Pasted image 20230214213456.png|400]]

What we're showing here is we can either rotate the camera 60 degree or rotate the object -60 degrees and achieve the same effect.

We obtain the same view from a camera at a certain location and orientation, by instead transforming the object. In CG we have no camera, only ==transformations==, but because the idea of a camera is so familiar we imagine a camera and thus graphics APIs allow us to use a "camera" but in fact we are simply using model transformations.

>[!warning] In Computer Graphics there is no camera, we simulate a camera by transforming the model (or the whole scene)
>This is very important ^^

When we say our camera's lens is at this position and it's facing this direction, the API has to workout the inverse and apply that to the scene!

Let's look at our camera coordinate system:

![[Pasted image 20230214215119.png|350]]

Here $\hat U,\ \hat F$ and $\hat S$ are the axis of the camera. $E$ is the position and orientation $C$, so we want to use these to derive what the camera system is.

To find $\hat F$, it's $E - C$ then normalise it. So calculate $F$ first ($E - C$), then normalize F to get $\hat F$
![[Pasted image 20230531202802.png|400]]


$\hat U$ is orthogonal to $\hat F$ and then normalise!
>[!definition]
>Orthogonal: Orthogonal refers to a geometric relationship between two objects or vectors that are perpendicular to each other. More specifically, two objects or vectors are orthogonal if their dot product is zero.

Finally $\hat S$ is orthogonal to both $\hat U$ and $\hat F$, which we can find by performing the cross product and then normalising.

But here we're assuming that $\hat U$ is orthogonal to $\hat F$ and what if it isn't? Well then our whole system will not work.. so what can we do? 

The solution is to decouple $\hat U$ and $\hat F$ by not making assumptions, we can derive $\hat F$ in the same manner = normalise($C - E$),  We then use $\hat V$ which we assume is orthogonal to help us find $\hat S$, $\hat S$ = normalise($\hat F \times \hat V$)

Then we use $\hat S$ and $\hat F$ to create $\hat U$ = normalise($\hat S \times \hat F$) and then we forget all about $\hat V$, this gives us our camera system.


##### Viewing Transformation (For the object)
So the camera coordinate system has axes $\hat S \hat U \hat F$ with origin $E$, there is a transformation $T_c$ which maps $XYZ$ to $\hat S \hat U \hat F$

So $T_c ^{-1}$ would translate & rotate the $\hat S \hat U \hat F$ camera to $(0,0,0)$ so we will apply $T_c ^{-1}$ to objects

![[Pasted image 20230214223546.png|500]]

So what is $T_c$ We firstly translate by $-E$

![[Pasted image 20230214223717.png|200]]

Rotate the camera axes to be coincident with the world axes with $\hat F$ aligned to $Z$

![[Pasted image 20230214223904.png|400]]
[DW about derivation, trust us x]

Thus $T_c ^{-1} = T2 \times T1$


In View Transformation, we have a window in our model and we have a viewport in our screen coordinates, and our View Matrix $M_{\text{view}}$ must translate the window frame to the viewport frame:
![[Pasted image 20230215085646.png|500]]

![[Pasted image 20230215085803.png|400]]

![[Pasted image 20230215090112.png|600]]

There may be some parts of an object that are clipped out of the viewport and window as so there are standard algorithms to deal with clipping lines and polygons

Also, because of duality of modelling and viewing, we cannot tell if camera was moved or the mdoel was moved just by looking at different images. It could have been either methods.