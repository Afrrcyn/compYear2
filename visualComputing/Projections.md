[[COMP27112]]
Where are we in the whole process?

![[Pasted image 20230221080544.png|600]]
![[Pasted image 20230221080601.png|600]]

### Planar Geometric Projections
We're trying to map from 3D world coordinates to 2D coordinates on the screen.

![[Pasted image 20230221080737.png|300]]

We're going to look at 2 types of projections that maintain straight lines - there are many other types of projections that distort things.

![[Pasted image 20230221081001.png|450]]

Parallel aren't used much in Computer Graphics.

##### Parallel
Informally, we have a 2D screen and the 3D object, we have a set of parallel lines coming off of many vertices of the 3D object and they collide with the 2D screen:

![[Pasted image 20230221081434.png|300]]

###### Orthographic
When we have the 2D plane being orientated to one facade of the object:

![[Pasted image 20230221081710.png|300]]

Undistorted view of the front of the building.
The projectors are perpendicular to the projection plane, and the projection plane is parallel to one of the planes.

![[Pasted image 20230221081756.png|400]]

So it can only really show 2 planes - so for instance, you may lose depth. 

So how do we create an orthographic view?

![[Pasted image 20230221082038.png|400]]

Place the plane on the $z = 0$ plane, and so it will be orthogonal to the $XY$ plane. And we can define a matrix to achieve this result:

![[Pasted image 20230221082328.png|300]]

This matrix is singular and so it has no inverse - this is because we are deleting the original $z$ value and so we cannot find the original...

###### Axonometric

![[Pasted image 20230221082509.png|450]]

When a projection plane is symmetric to a plane it must sit at $45 ^\circ$  to the plane, so for Isometric, the projection plane sits at $45^\circ$ to all planes. Dimetric is similar.

In all the cases, we can draw a normal vector from the projection plane and it will intersect the origin of the axis.

###### Oblique

We have no constraints on the projection plane, it can have any orientation... and allows us to see all 3 axis, and lengths and angles may get distorted.

>[!Note]
>In computer graphics, perspective projections are common, for their sense of realism

##### Perspective

![[Pasted image 20230221095615.png|400]]

The centre of projection is a point, and all projectors converge onto that point. (Parallel lines will converge (?))

![[Pasted image 20230221101924.png|600]]

- 1 point perspective, has a single vanishing point, lines that should be parallel, if we extend them (like the foundation) if we extend them it should extend to a single point.
- 2 point, if we take lines that are parallel, if we extend them, we end up with a pair of vanishing points.
- 3 point, extending parallel lines makes 3 vanishing points.

So how do we compute where a point should be projected to in a perspective projection?

![[Pasted image 20230221102716.png|300]]

The point $(x,\ y,\ z)$ will converge to the origin, and goes through the point $(x_p,\ y_p,\ z_p)$ and then hits the origin, and we set the projection plane to be $z_p = d$, so how do we work out $x_p$ and $y_p$.

Let's look at the top down view:

![[Pasted image 20230221104235.png|300]]

We can set up 2 similar triangles in this view.  Triangles:$(\text{Origin})-(x_p,\ y_p,\ z_p)-(x_p,\ y_p,\ 0)$ and $(\text{Origin})-(x,\ y,\ z)-(x,\ y,\ 0)$

![[Pasted image 20230221105621.png]] (Remember that $z_p = d$)

And we can do exactly the same thing in the $YZ$ plane:

![[Pasted image 20230221142402.png|300]]

![[Pasted image 20230221143042.png]]

This can be expressed a matrix:

![[Pasted image 20230221143137.png|400]]

Multiplying this out:
 - $x' = x$
 - $y' = y$
 - $z' = z$
 - $w' = z/d$

But this makes no sense - it looks nothing like it's supposed to, however we should remember that this is a homogeneous matrix and the $w'$ should be 1, and so we have to divide $x',\ y',\ z' \text{ and } w'$ by $w'$

![[Pasted image 20230221143640.png|400]]

##### Summary
We derived the matrix to perform a 1-point projection where the projection is parallel to the $XY$ plane.

![[Pasted image 20230412063803.png|450]]

![[Pasted image 20230412063825.png|450]]

### Viewing Volumes and Clipping

- So having specified the position and orientation of the camera
- We now have to describe the field of view of the camera by defining a 3D view volume.
- Only those parts of 3D objects which lie inside the view volume are displayed - all objects are clipped against the view volume.

##### Parallel
![[Pasted image 20230221193909.png|300]]

We have the Camera, with $\hat F$ pointing into the scene. So we have 2 planes, that define the left, right, top and bottom of the field of view - near and far plane, they are orthogonal to the axis.

![[Pasted image 20230221194921.png|300]]

Constructor to create an Orthographic Camera (image above)
![[Pasted image 20230221195113.png]]


##### Perspective
View volume = ==Frustum==
>[!Definition]
>A frustum is a geometric shape that is created when you take a 3-dimensional object (usually a cone or pyramid) and slice off the top portion. The resulting shape has two parallel bases that are different sizes, connected by a lateral surface. The lateral surface of a frustum is a truncated cone or pyramid, depending on the original shape of the object.


![[Pasted image 20230221194729.png|300]]

![[Pasted image 20230221195239.png|450]]
Frustum is defined by the near plane and far plane. Also by a field of view angle, and an aspect ratio (width and height of the frustum)

Constructor for Perspective camera
![[Pasted image 20230221195301.png]]


###### Problem with perspective projection:
A projection matrix transforms a 3D point to a 3D point with W != 1 for example for a 1-point perspective projection. 

![[Pasted image 20230221201236.png|400]]
So we need to divide through $w$ to get $(x_p,\ y_p,\ z_p)$

$z_p = d$ which means we have lost any idea of the original 3D objects z-coordinates, they have been set to d...

This is unhelpful as we want to keep the z-depth information to do hidden-surface removal. Solution: Projection Normalisation.

>[!info] Projection Normalisation
>- We need a perspective transformation which preserves depth information.
>- Suppose we have a particular perspective transformation expressed by frustum $F$ and matrix $M$
>- It can be shown that we can derive a new transformation matrix $PN$, based on $M$ that distorts $F$ into a cube
>- ![[Pasted image 20230221203716.png|300]]
>- Transforming our model by $PN$ and then taking an orthographic projection produces exactly the same result as performing our original perspective transformation $M$ with one difference:
>
>The $z$ depth is preserved
>![[Pasted image 20230221203804.png|400]]

##### Clipping
Taking the object we're trying to render and it's taking it's view volume and it's essentially removing anything which is outside of the view volume.
Clipping can be summarised by the following image:

![[Pasted image 20230221203856.png|450]]

So the polygon or the diagram was first overlapping and extending beyond the outline of the clipping region. Clipping operation will trim the polygon so the entire polygon is within the viewing area of the clipped region.


