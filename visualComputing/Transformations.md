>[!warning] Question, Ask in Lecture!
>[[#^8ce4b4|Here]], doesn't this transformation achieve nothing and therefore what's the point in doing it?

Both Coordinates and Vectors can be represented by a triple of $x,\ y,\ z$  values. In computer graphics we usually write these out in the format: $$\begin{align} V &= \begin{bmatrix} x \\ y \\ z \end{bmatrix} \end{align}$$
So if we have the above vector for an an object, then we can say the spatial relationship between the origin and the melon is described by vector $V$

> [!Note]
> We could easily have used a row vector for the coordinates, they both transpose to each other but it's not of importance. 

##### Translate
- Simply moving an object: $$\begin{align} x' = x + \text{t}x \\ y' = y + \text{t}y \\ z' = z + \text{t}y \end{align}$$
![[Pasted image 20230208105727.png|500]]

##### Scaling
- Simply multiply each dimension of the object by some scalar factor. $$\begin{align} x' = x. \text{s}x \\ y' = y .\text{s}y \\ z' = z .\text{s}y \end{align}$$![[Pasted image 20230208110039.png|500]]
##### Rotation (2D)
- We want to rotate point $P$ about the origin by angle $\theta$ 

![[Pasted image 20230208110707.png|500]]

> [!Note] 
> That: The positive direction of a rotation is anti-clockwise!

So how do we calculate the new position? Let's draw out an example.

![[Pasted image 20230208111038.png|500]] 

$$\begin{align} x = R\cos \phi \\ y = R\sin \phi \\ x' = R\cos(\theta + \phi) \\ y' = R\sin(\theta + \phi)  \end{align}$$Note that also: $$\begin{align} x' = R\cos \phi \cos\theta - R\sin \phi \sin\theta \\ y' = R \cos \phi \sin \theta + R \sin \phi \cos \theta \end{align}$$ And we can substitute $x$ and $y$ into the above equations giving us:  $$\begin{align} x' = x\cos \theta - y \sin \theta \\ y' = x \sin \theta + y \sin \theta \end{align}$$
![[IMG_FDC27F486036-1.jpeg|500]]
[The image above, is just me visually trying to understand how rotation works and how we figured out the equation above]

Note that if we are not rotating around the origina but rather a point, say point $Q$ = $(x_1, y_1)$ then we simply translate all points (by the same translation) such that point $Q$ becomes the origin - we then perform the rotation and then translate the origin back to $Q$.

##### Rotations (3D)
- for Rotation in the Z axis, on the vector $\big [x,\ y,\ z \big]$, the $z$ value stays the same and so, it's simply a 2D rotation on the values $x$and $y$ thus: $$\begin{align} x' = x\cos \theta - y \sin \theta \\ y' = x \sin \theta + y \sin \theta \\ z' = z \end{align}$$

##### Rotations (3D) around a vector
- In 3D we may want to rotate or scale about an arbitrary axis vector...
- You'll see more later x

### Matrix
All of these transformations require different operations and formats and it would be nice if we could have a consistent system? Answer: Matrices, wow, who knew?

##### Scaling
$$\begin{align} \begin{bmatrix} x' \\ y' \\ z' \end{bmatrix} = \begin{bmatrix} 2 & 0 & 0 \\ 0 & 4 & 0 \\ 0 & 0 & 3 \end{bmatrix} . \begin{bmatrix} x \\ y \\ z  \end{bmatrix} = \begin{bmatrix} 2x \\ 4y \\ 3z \end{bmatrix}\end{align}$$ Here, in this example, the $x$ is scaled by the factor 2, the $y$ by 4 and $z$ by 3

##### Rotation (3D) around Z axis
$$\begin{align} \begin{bmatrix} x' \\ y' \\ z' \end{bmatrix} = \begin{bmatrix} \cos \theta & -\sin \theta & 0 \\ \sin \theta & \cos \theta & 0 \\ 0 & 0 & 1 \end{bmatrix} . \begin{bmatrix} x \\ y \\ z  \end{bmatrix} \end{align}$$ 


##### Translation?
- Doesn't work... sorta we can do this for the vector $\big [  x,\ y,\ z \big]$: $$\begin{align} \begin{bmatrix} x' \\ y' \\ z' \\ w' \\ \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 & \text{t}x \\ 0 & 1 & 0 & \text{t}y \\ 0 & 0 & 1 & \text{t}z \\ 0 & 0 & 0 & 1 \end{bmatrix} . \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} \end{align}$$
$x' = 1*x + \text{t}x$
$y' = 1*y + \text{t}y$
$z' = 1*z + \text{t}z$
$w' = 1$

###### Homogeneous coordinates
- In order to use a consistent matrix representation for all kinds of linear transformations, we've had to add an extra coordinate $w$ to our 3D coordinate $\big [  x,\ y,\ z \big]$
- This coordinate form $\big [  x,\ y,\ z,\ w \big]$ is called homogeneous coordinates, but what is w and where is it from? Well, it's the 4th spatial dimension, when $w = 1$ we can ignore it, however if it's $w \neq 1$ we have to normalise it.

##### 3D scaling (with homogeneous)
$$\begin{align} \begin{bmatrix} x' \\ y' \\ z' \\ 1 \\ \end{bmatrix} = \begin{bmatrix} \text{S}x & 0 & 0 & 0 \\ 0 & \text{S}y & 0 & 0 \\ 0 & 0 & \text{S}z & 0\\ 0 & 0 & 0 & 1 \end{bmatrix} . \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} \end{align}$$

##### 3D Rotation Matrix (around x-axis)
$$\begin{align} \begin{bmatrix} x' \\ y' \\ z' \\ 1 \\ \end{bmatrix} = \begin{bmatrix} 1 & 0 & 0 & 0 \\ 0 & \cos \theta & -\sin\theta & 0 \\ 0 & \sin\theta & \cos\theta & 0\\ 0 & 0 & 0 & 1 \end{bmatrix} . \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} \end{align}$$

##### 3D Rotation Matrix (around the Y axis)
$$\begin{align} \begin{bmatrix} x' \\ y' \\ z' \\ 1 \\ \end{bmatrix} = \begin{bmatrix} \cos\theta & 0 & -\sin \theta & 0 \\ 0 & 1 & 0 & 0 \\ \sin\theta & 0 & \cos\theta & 0\\ 0 & 0 & 0 & 1 \end{bmatrix} . \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} \end{align}$$

##### 3D Rotation Matrix (around the Z axis)
$$\begin{align} \begin{bmatrix} x' \\ y' \\ z' \\ 1 \\ \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 & 0 \\ \sin\theta & \cos \theta & 0 & 0 \\ 0 & 0 & 1 & 0\\ 0 & 0 & 0 & 1 \end{bmatrix} . \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} \end{align}$$

##### Composite Transformations
Remember earier when we spoke about rotating a vector around a point as opposed to the origin and that we would translate the point to the origin, perform the rotation and translate back, well we can perform all these transformations separately - or, we can turn all the operations into one matrix and apply one operation:
$$\begin{align} P' = T_2.T_1.P \\ T_c = T_2. T_1 \\ 
P' = T_c.P\end{align}$$
By multiplying matrices together we get the affect of applying both of them to a vector, note however that the order in which we multiply and apply them to a vector is important. So it's non commutative!

We might also need this for when scaling, if we do not scale a point about the origin, we will end up translating it as well and we may not want to do that, so we translate to origin, scale and translate back, this can be made into 1 transformation using 3 operations, with operation 3 undoing the affects of operation 1.

What if we want to undo a transformation? Well we simply find the inverse of a matrix, and that undoes what the matrix did. Say for example: $$\begin{align} A = \begin{bmatrix} 1 & 0 & 0 & \text{t}x \\ 0 & 1 & 0 & \text{t}y \\ 0 & 0 & 1 & \text{t}z \\ 0 & 0 & 0 & 1 \end{bmatrix}  \end{align}$$ (Note this is a Translation Matrix, we can then use the inverse) $$\begin{align} B = \begin{bmatrix} 1 & 0 & 0 & -\text{t}x \\ 0 & 1 & 0 & -\text{t}y \\ 0 & 0 & 1 & -\text{t}z \\ 0 & 0 & 0 & 1 \end{bmatrix}  \end{align}$$
And thus $A.B = I$ where $I$ is the identity function, this makes sense logically as the 2 matrix operations should cancel each other and thus it would have the same effect as multiplying by the Identity Matrix.

>[!note] This is the definition of an inverse
>If $A$ multiplied by $B$ is the identity function $I$, then $A$ and $B$ are inverses. BUT please do note here that not all Matrices have an inverse.

##### Arbitrary Rotation on a vector
Step 1: We shift vector $A$ until it passes through the origin, this is transformation $M_1$, call this new vector $A_1$

![[Pasted image 20230209203037.png|400]]

Step 2: Rotate $A_1$ about the X-axis util it lies in the XY place, this transformation is known as $M_2$ and produces the vector $A_2$

![[Pasted image 20230209203237.png|400]]

Step 3: Rotate $A_2$ about the Z axis until it is coincident with the X-axis, this is transformation $M_3$ and produces a new vector $A_3$

![[Pasted image 20230209203820.png|400]]

Step 4: Perform the desired rotation about the X-axis by $\theta$ (which we know how to do!) This is transformation $M_4$ ^8ce4b4

![[Pasted image 20230209204043.png|400]]

> Now undo all the other transformations in reverse order:

Step 5: Apply $M_3 ^{-1}$
Step 6: Apply $M_2 ^{-1}$
Step 7: Apply $M_1 ^{-1}$

Therefore $P' = M_1 ^{-1} . M_2 ^{-1} . M_3 ^{-1}. M_4 . M_3 . M_2 . M_1 . P$ 


### Transformation in OpenGL
- OpenGL maintains two transformation matrices internally;
	- The model-view matrix used for transforming the geometry you draw and specifying the camera.
	- The projection matrix, used for controlling the way the camera image is projected onto the screen (see later x)
- Note that this is what the vertexShader does!
$P_{drawn} = \text{ProjectionMatrix} \times \text{Model-ViewMatrix} \times P_{specified}$

### Essential Vector Geometry!
- ==Addition== - Subtraction works in much the same way!
$$\begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} + \begin{bmatrix} x' \\ y' \\ z' \\ 1 \end{bmatrix} = \begin{bmatrix} x + x' \\ y+y' \\ z+z' \\ 1 \end{bmatrix}$$
- ==Scalar Multiplication==
$$\alpha\begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix} = \begin{bmatrix} \alpha.x \\ \alpha.y \\ \alpha.z \\ 1 \end{bmatrix}$$
- ==Vector Magnitude==
$$V = \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}$$
$|V| = \sqrt{x^2 + y^2 + z^2}$

- ==Normalisation== - makes a vector into a unit vector.
$$V = \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}$$
$|V| =  \sqrt{x^2 + y^2 + z^2} = L$
$$\hat V = \begin{bmatrix} x /L \\ y/L \\ z/L \\ 1 \end{bmatrix}$$

- ==Vector Multiplication==:
There are 2 methods, both are essential operations in 3D graphics.
- Dot Product - known as the inner product and gives a scalar
$$\begin{bmatrix} x_1 \\ y_1 \\ z_1 \\ 1 \end{bmatrix}.\begin{bmatrix} x_2 \\ y_2 \\ z_2 \\ 1 \end{bmatrix} = (x_1.x_2)+...+(z_1.z_2)$$

> [!Note]
> If two vectors are normalised then their dot product is the cosine of the angle between them.
> $\cos\theta = \hat V_1. \hat V_2$
> ![[Pasted image 20230210001755.png|300]]

- Cross Product - known as outer product and gives a vector 
$$\begin{bmatrix} x_1 \\ y_1 \\ z_1 \\ 1 \end{bmatrix}.\begin{bmatrix} x_2 \\ y_2 \\ z_2 \\ 1 \end{bmatrix} = \begin{bmatrix} y_1\times z_2 - z_1 \times y_2 \\ z_1\times x_2 - x_1 \times z_2 \\ x_! \times y_2 - y_1 \times x_2 \\ 1 \end{bmatrix}$$ [Don't worry about remembering this, but do note that the cross product is perpendicular to both of the original two vectors!]


### Shaders
Remember the [[OpenGL#Pipeline|graphics pipeline]], let's look at the two things you can program - vertex shaders and fragment shaders.

##### Vertex Shader
- All shapes are made from vertices, and the vertex shader receives each in turn.
- Performs some sort of processing - and sets the values for `gl_position` (a vec4) which is where the vertex will appear on screen.
- The information eventually reaches the fragment shader for this vertex.

![[Pasted image 20230210091554.png|400]]

Minimal Vertex Shader:
```javascript
void main() {
	gl_Position = projectionMatrix *
				modelViewMatrix *
				vec4(position, 1.0)
}
```
> three.js provides both, `projectionMatrix` and `modelViewMatrix`

##### Fragment Shader
- How to colour an object that has vertices
- It must set a value for `gl_FragColor` (a vec4) which is the final colour of the fragment.

![[Pasted image 20230210100225.png|400]]

![[Pasted image 20230210100343.png|400]]

Simplest Fragment Shader
```javascript
void main() {
	gl_FragColor = vec4(1.0, 0.0, 1.0, 1.0)
	// RGBa
}
```

![[Pasted image 20230210101223.png|500]]

##### Uniforms 
- A way of getting data into both the vertex shader and fragment shaders *(hence called uniform)*
- Stay constant throughout the frame *(hence called uniform)* we're rendering (it can be changed between frames)
- Usually single values - light positions, material colours, ...

##### Attributes 
- Passes into the vertex shader, only, and are applied to individual vertices *(hence it's the attributes... of all vertex)*
- Could be an array of attributes - one per vertex.

##### Varyings 
- Values computed in vertex shader and then it's values are passed into the fragment shader.
- Must declare a varying variable of the same name and type in both shaders.

![[Pasted image 20230210103504.png|600]]![[Pasted image 20230210103941.png|600]]![[Pasted image 20230210102241.png|600]]
![[Pasted image 20230210102448.png|600]]







