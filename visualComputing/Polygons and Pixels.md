[[COMP27112]]

Polygons are the most common building block for 3D graphics, with Triangles being the most commonly used.

>[!Definition]
>Mesh: Mesh refers to a collection of vertices, edges, and faces that define the shape of a 3D object.

What is a polygon?
- Many sided shape...
- We can equally represent polygons via their vertices or edges, vertices are preferred.

> Polygon is just the space that is bounded by the vertices

OpenGL needs convex polygons, all it's interior angles are less than 180 degrees:

![[Pasted image 20230214081405.png|400]]

A shape with more than 180 degrees angle is called concave. If we have a shape with more than 180 degree angle (like the one on the right, in the above picture) we will use an approach called ***Tessellation***

> [!Definition]
> Tessellation is the process of breaking down a 3D surface into smaller geometric shapes, such as triangles or quadrilaterals, to enable smoother and more detailed rendering of the surface.

![[Pasted image 20230214081728.png|500]]

So we introduce a couple more vertices on the edges of the shapes to ensure that there are no more reflex angles.

##### Surface Normal
- The surface normal is a vector perpendicular to the plane of the polygon. We can use this to give a polygon a distinguishable front and back, and also to describe it's orientation in a 3D space.
- The convention is that the surface normal comes out of the front of the polygon, and of course the other side will be the back
- Orientation is an essential property used extensively in lighting calculations, collisions, culling, ...

![[Pasted image 20230214082045.png|500]]

##### Cross Product
is a vector, defined as follows: $$\begin{bmatrix} x_1 \\ y_1 \\ z_1 \\ 1\end{bmatrix} \times \begin{bmatrix} x_2 \\ y_2 \\ z_2 \\ 1\end{bmatrix} = \begin{bmatrix} y_1\times z_2 -  z_1 \times y_2 \\ x_1 \times z_2 - z_1 \times x_2 \\ x_1\times y_2 - y_1 \times x_2 \\ 1\end{bmatrix}$$
For 2 vectors, their cross product is a third vector perpendicular to both of them.

![[Pasted image 20230214082453.png|400]]

Finding the surface normal:
1. Choose a pair of sequential edges $E_1$ and $E_2$ and compute their vectors
2.  Invert the direction of the first so now they emanate (emerge) from their shared vertex. (does this makes sense? - if not, look at diagram and see that inverted $E_1$ starts at same vertex as $E_2$)
3. Their Cross product gives the Surface Normal $N$, where $N = E_1 \times -E_2$ 
4. We then almost always have to normalise $N$ (make it's length 1)

![[Pasted image 20230214090037.png|400]]

##### Representing scenes with polygons
When we render a scene with polygons we refer to it as a polygon soup as it's a mis-mash of polygons of all different colours..

The problem with the polygon soup is that 
- it's a huge waste of storage space, most models contain surfaces not individual polygons so we could share vertices rather than replicate them per polygon.
-  How can we differentiate the polygons that belong to the cow or the table? So we lose the semantics and this makes interaction with the model very hard.

### Polygon Mesh
Polygons that link up with adjacent polygons, so we're representing multiple polygons and so we reduce the number of separate vertices.
- Note the problem of semantics still remains.

How can we make polygon meshes?

##### Triangle Strips
- Simply by adding one more vertex you get a whole new triangle - so less space used up as less repeated vertices as they're shared.

![[Pasted image 20230214091715.png|400]]

##### Triangle Fan

![[Pasted image 20230214094049.png|400]]

- One vertex is shared among all triangles
- So it's a collection of linked triangles, very widely used as it's efficient. 
- So $N$ linked triangles can be defined as $N+2$ vertices compared with $3N$ vertices if each triangle were separately defined.

##### Quadrilateral Strips
![[Pasted image 20230214094605.png|400]]

- We don't just need to use triangles. For each further quadrilateral, we just add two more vertices.

##### Quadrilateral Meshes
- Collection of linked quadrilaterals
- Used in terrain modelling and for approximating curved surfaces.

![[Pasted image 20230214095019.png|400]]

- $N \times M$ quads can be defined using $(N+1)\times(M+1)$ vertices compared with $4 \times M \times N$ separate vertices.

This can be tessellated into triangles:

![[Pasted image 20230214095421.png|400]]

Allows for more flexible surfaces.

### From the model to the display

![[Pasted image 20230214095744.png|400]]

We're going to change example for simplicity:

![[Pasted image 20230214095920.png|400]]

So we have to scan convert a line:

![[Pasted image 20230214095958.png|400]]

The blue lines are not straight simply because we're working in the digital world, as we can only represent them with pixels and are limited and so the line is approximated, like everything in computer graphics is approximated.

Scan Conversion is solved by Bresenham's Algorithm, which starts at one end of the line, and you look at how the the line should have as you increase the $x$ value by 1 (as seen below!)

![[Pasted image 20230214100204.png|400]]

If I increment the y value by the real value it should be increased by, which pixel am I closest to.

So here are the more formal steps:
1. define equation of the line $y = mx +c$
2. As we move horizontally $x$ changes by 1 pixel
3. So $y_{n+1} = y_n + m$
4. Round $y_{n+1}$
5. Need to swap $x$ and $y$ according to gradient of the line.
6. Bresenham (1965) developed a fast algorithm using only integer arithmetic that is still used today!

So we scan converted a line, how do you scan convert a polygon is the next question?

- There are many approaches... we'll look at 2.
- The polygon has been transformed by the viewing pipeline, so we know it's  $(x, y, z)$ vertex coordinates  in screen space.
- The $(x, y)$ coordinates corresponds to a pixel position.
- The $z$ coordinate is a measure of the vertex's distance from the eye (or camera), we won't use this information yet!

##### Approach 1
To scan convert a triangle, we scan convert the edges, and then after finishing the edges, fill in the dots!

Yeah, this approach above is naïve and not used...

##### Approach 2
We're gonna look at the sweep-line algorithm

![[Pasted image 20230214101713.png|400]]

We start at $x1, y1$ and then we jump down a scanline and compute the start and end of the x coordinates of that scanline and thus we can fill that line in. 
What we can also do is find out how much the $x$ coordinate increments for each scanline.  (Found using Bresenham's algorithm) and so knowing the gradient of the lines, we can work out the change in $x$ making it easier to find the start and end of scanlines, so the efficiency comes from the fact we only find the gradient once.

##### Hidden Surface Removal
- Viewing the world from a particular viewpoint, which parts of the world can be seen and which parts can we not see, because other objects block them
- There are 2 fundamental approaches to solving this problem.
	- Work in 3D world space, we can work it out geometrically in 3D and work it out. (?)p
	- Work in a 2D display space, during scan-conversion, whenever we generate a pixel $P$ we determine whether some other 3D objects nearer to the also also maps to $P$, this is now the standard approach.

###### Z-buffer (aka Depth-Buffer)
We introduce a new data structure called the Z-buffer
- For every pixel in the display memory, there is a corresponding entry in the Z-buffer
- The Z-buffer is used to keep a record of the z-value of each pixel.

![[Pasted image 20230214105001.png|400]]

![[Pasted image 20230214105511.png|600]]

this works quite nicely except for when Z values of different colours are very close together, where rounding errors can cause problems like stitching:
![[Pasted image 20230214134814.png|400]]

Especially unpleasant when used in animation..


### Modelling with Multiple Objects

##### Structured Model:

![[Pasted image 20230214135355.png|300]]

Any object can be represented using a structured polygon, it needs to maintain some sort of hierarchy.

- A model can be made up of Object(s)
	- Each Object is made up of Surface(s)
		- Each Surface is made up of Polygon(s)
			- Each Polygon is made up of Edge(s)
						   - Each Edge is made up of pairs of Vertices

General Polygon Mesh:
- Made up of Surfaces and Polygons and Edges... but we want to have some sort of generalised way of representing general shaped objects and to do that we need:

##### Mesh Data Structure:

![[Pasted image 20230214135840.png|300]]

So our mesh data structure has 3 lists, ==A list of all the nodes== in our data structure, ==a list of all the edges== in our data structure (edges are just defined by 2 vertices - so it indexes within the list of vertices) and thirdly we have a face list, then there's ==a list of faces==, each face is defined by a list of 3 edges (dealing with triangles).

![[Pasted image 20230214141730.png|450]]


##### File Formats
obj file formats:
- List of vertices: $\bf{v}$ $x\ y\ z$
- List of Vertex normals: $\bf vn$ $x\ y\ z$
- List of vertex texture coordinates: $\bf vt$ $s\ t$
- List of faces: $\bf f$ $v1/vt1/vn1\ \ v2/vt2/vn2\ ...$
- List of groups $\bf g$ $f1\ f2\ f3$ which is a group of faces (?)

