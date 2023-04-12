[[COMP27112]]

How do we take the local illumination model from last lecture:

![[Pasted image 20230228075158.png|500]]

And we want to use this for a triangle mesh - so how do we colour a surface.

Three shading methods:
- Flat (constant)
- Gouraud (intensity)
- Phong (normal-vector)

![[Pasted image 20230228075255.png|400]]

##### Flat
Known as constant shading.

Take one of the vertices of the polygon, compute the surface normal and compute the local illumination model and find the colour of the vertex and then colour in the whole triangle the same colour.

Each polygon is uniformly coloured according to it's orientation, so you can clearly see the mesh.

![[Pasted image 20230228075448.png|400]]

The Mach Band effect makes the boundaries far more visible.. which is that humans are really good at seeing edges:

![[Pasted image 20230228075614.png|300]]


##### Gouraud
Also known as intensity shading. Uses interpolation to smooth out the discontinuities between polygons.

So we calculate the surface normal at each vertex of a polygon.

![[Pasted image 20230228080101.png|400]]

If we know a number of polygons meet at a vertex we simply have to take an average of their normals:

![[Pasted image 20230228080132.png|300]]

This gives us a surface normal. And we do this for all vertices and so we apply the local illumination model to all averaged out surface normals. And we interpolate the colour of the polygon.

![[Pasted image 20230228080231.png|500]]

We do this for each scanline.

![[Pasted image 20230228080440.png|400]]

One problem that we have, is that at the boundary of the object - an edge that is adjacent to nothing clearly shows it's boundaries - look at the eyes. or even the nose hole.

Implementing Gouraud:
- We need to optimise the computation as much as possible
- For each scanline we compute the colour increment between pixels:

![[Pasted image 20230228080803.png|400]]

- Similarly we can also optimise by computing $C_{\text{right}}$ and $C_{\text{left}}$ incrementally
- (This is almost like calculating the colour gradient so that you don't have to interpolate the colour anymore and simply use last value + gradient)

Gouraud Problems:
- Specular highlights may be distorted or averaged away altogether (because Gouraud shading averages between vertex colours)
- Mach Banding may still be visible.
- Although sometimes edges are averaged out as well...

##### Phong 
Also known as Normal Vector Interpolation.

Instead of interpreting colours, phong suggested interpolating normals instead.
We interpret normal vectors along the scanline, compute the illumination model for each pixel.

![[Pasted image 20230228081451.png|500]]

Again you can pre-compute increments for vectors to help with efficiency

![[Pasted image 20230228081652.png|400]]

Has fixed the specular shading.

![[Pasted image 20230228081721.png|600]]

Rendering Expense..
- Roughly our illumination model takes about 60 Floating Point Operations per colour per pixel.
For Gouraud-shaded triangle that's about 180 flops then about 2 per pixel...
For Phong-shaded triangle, that's about 60 flops for every pixel...

### Surface Detail
We will look at two things:
- Texture Mapping
- Bump Mapping

##### Texture Mapping
The simple way is imaged based - wrapping an image on the surface
The other way is a procedural way that computes the texture pattern as it's drawn on the shape. Very limited in what kind of texture mapping you can achieve and thus we won't talk about this...

Defining a Texture
A Texture can be thought of as an image made up of ==Texels== (Pixels = Picture image, texels = texture image)... We can set up an axis $u$ and $v$ for the texture.

![[Pasted image 20230228083035.png|400]]

We map texture $u,\ v$ coordinates with the vertices. We then use Interpolations with the scanlines to work out the texel that should be placed at that pixel.
Note we will also have to blend the final colour of the polygon - found using gouraud or phong. And then blend this with the colour of the texture.

![[Pasted image 20230228083322.png|500]]

Problems:

- Seams - having two textures that are put next to each other - it may look the textures look discontinuous thus shows it's seam - one way to fix this is to ensure the texture wraps around
- Resolution Mismatch - what happens if the resolution of the texture map does not match the resolution of the polygon we're trying to render it to..?

![[Pasted image 20230228083650.png|500]]

There are 3 cases:
- Pixel Resolution == Texture Resolution
- Pixel Resolution > Texture Resolution (1)
	- i.e. Pixels are smaller than texels
- Pixel Resolution < Texture Resolution (2)
	- i.e. Pixels are bigger than texels

###### For Case (1):
(Pixel Resolution > Texture Resolution)
- Multiple pixels on the polygon would map to the same texel on the polygon:

![[Pasted image 20230228083906.png|400]]

Method 1 - basically just leave it - not good and leaves image block
Method 2 - We find the fractional coordinate that the pixel refers to - find the 4 nearest neighbour (like a quad tree...) and average them.

![[Pasted image 20230228084148.png|400]]![[Pasted image 20230228084306.png|600]]

Of course there's a cost implication with doing bilinear filtering...

###### For Case (2)
(Pixel Resolution < Texture Resolution)

![[Pasted image 20230228084530.png|400]]

Two pixels adjacent may map to two texels not at all adjacent to each other.

One solution is called MipMap Filtering
- The further away from the viewpoint the less detail we need.
- So we use a set of texture maps and select which map to use according to the distance of a pixel from the viewer.

![[Pasted image 20230228084815.png|500]]

So we have made multiple copies of different resolutions. Although in terms of storage it takes about twice the amount of storage compared to normal...

So now we have many choices to choose from to allow for a good texture to pixel ratio.

![[Pasted image 20230228085131.png|400]]

Look at the tiles further away..

![[Pasted image 20230228085229.png|400]]

Now it just looks more blurred...

Textures as illumination:
- Off-line, pre-compute accurate diffuse illumination of the scene using a global model (radiosity)
- Save rendered surfaces as textures - called lightmaps
- In real-time, apply lightmap textures and also actual surface textures.

##### Bump Mapping

How do we model the shape of the surface such that when we apply light it gives a bumpy look

![[Pasted image 20230228090033.png|400]]

We deform the surface such that the surface normals are changed - bump mapping does not alter the colour. Lighting will then allow it to appear like it's bumpy...

![[Pasted image 20230228090150.png|400]]


![[Pasted image 20230228093112.png|400]]

We invent $\hat N_U$ and $\hat N_V$ such that they are orthogonal to the surface normal $\hat N$

![[Pasted image 20230228093156.png|400]]

We can then multiply these vectors by some scalar.

![[Pasted image 20230228093320.png|400]]

So now we can make a new surface normal (also note we must normalise this new vector as well)

What we do is we take a texture as a bump map to derive $b_U$ and $b_V$ - we can use the texel colours to encode the bumpiness.

![[Pasted image 20230228093737.png|400]]

You can see the blacks show a low amplitude and the whites are the highest amplitude.

![[Pasted image 20230228093814.png|400]]

