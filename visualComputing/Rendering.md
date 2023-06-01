[[COMP27112]]

>[!info] Recap
>During rasterisation we:
>- Choose the best set of pixels
>- Apply the z-buffer to remove hidden parts 
>- Apply an illumination model and shade smoothly

>Rasterisation: Rasterization is a process used in computer graphics to convert vector graphics or 3D models into a raster image, which is a collection of pixels or dots that can be displayed on a computer screen or printed on paper.


In `three.js`
- Achieved by the material constructor:
	- ShaderMaterial
	- MeshBasicMaterial
	- MeshLambertMaterial
	- MeshPhongMaterial
	- and so on....

So rendering is taking something like this:
![[Pasted image 20230221205444.png|400]]
The Raw Geometry and Producing something like this:
![[Pasted image 20230221205621.png|400]]

Let's start with Illumination and more specifically...

### Global and Local illumination:

- Locally: we treat objects in a scene separately from any other object.
- Globally: We treat all objects together and model the interactions between objects. Illumination is reflected then to other objects

We approximate the interaction of light and matter:
- The standard local model is a simple approximation
- adding per-material algorithms (usually called shaders) gives better results
- the global model is better than the local model.

Route Map
- light intensity only (no colour)
- ambient illumination
- diffuse reflection
- a positional light source
- specular reflection
- coloured lights and surfaces.

![[Pasted image 20230221231713.png|400]]

Diffuse reflection is absorption and uniform radiation
- some wavelengths are absorbed, some are reflected
- a green object looks green because it only reflects green (picks up colour in the transparent layer)

Specular reflection is reflection at the surface/air interface
- to a first approximation, the colour of the specular reflection is the same as the light source

![[Pasted image 20230221232653.png|300]]

A perfect diffuse reflection:

![[Pasted image 20230221233144.png|400]]

other diffuse

![[Pasted image 20230221233241.png|400]]

Perfect specular reflector
![[Pasted image 20230221233351.png|400]]
Note that in a perfect specular reflection, angle of incidence is equals to angle of reflection

![[Pasted image 20230221233441.png|400]]

##### Developing a Model

let's look at Diffuse reflection and different types of illumination
1. Ambient illumination
3. Point illumination source at infinity (directional illumination)
4. Point illumination source in the scene


###### Ambient
Let's look at ambient... The light bulb gives off rays of lights and together these rays can be seen as uniform illumination
>![[Pasted image 20230222182950.png|300]]
>![[Pasted image 20230222183009.png|300]]
>![[Pasted image 20230222183146.png|300]]

- If monochrome intensity of ambient light is $I_a$
- Amount of ambient light diffusely reflected from a surface is: $$I_{\text{ambient}} = K_a I_a$$
- where $K_a$ is the ==ambient reflection coefficient== of the surface and $0 \leq k_a \leq 1$

>[!info] Local Illumination Model v1
>$$I = K_a I_a$$

Wireframe versus Ambient
![[Pasted image 20230222184147.png|400]]

You lose all sense of surfaces... as all surfaces are reflecting equally....

If we keep $I_a$ constant and vary $K_a$

![[Pasted image 20230222184257.png|300]]

If we compare a model to the real photo and subtract them we get a difference image...

![[Pasted image 20230222184551.png|500]]![[Pasted image 20230222184610.png|500]]

Next level of complexity...

###### Directional Lighting

![[Pasted image 20230222184744.png|400]]

Let's simplify things down...

![[Pasted image 20230222184902.png|400]]

and concern ourselves with only the direction.

![[Pasted image 20230222184944.png|400]]

Where:
- $\hat N$ is the surface normal
- $\hat L$ is the direction of light source (why did we reverse the direction? Because we are going to find to do the inner product to find $\cos \theta$)
- $\theta$ is angle of incidence
- All vectors are normalised xx

> [!note] Lambert's Law
> If we have light source of intensity $I_p$ 
> Then the effective intensity $I_e$ is $$I_e = I_p \cos \theta$$

Note when $\theta = 0$ we have that $I_e = I_p$ and this makes sense as we're practically shining a light right above it, however as $\theta \rightarrow 90^\circ$ we find that $I_e \rightarrow 0$ which also makes sense to some degree.

###### Diffuse Reflectivity
- We describe the diffuse reflectivity of a surface by assigning it a value $k_d$
- $k_d$ is the diffuse reflection coefficient of the surface $0 \leq k_d \leq 1$
- Amount of diffusely reflected light is:
	- $I_{\text{diffuse}} = I_pk_d\cos\theta$
	- $I_{\text{diffuse}} = I_pk_d(\hat N . \hat L)$
>[!Recall]
>$cos\theta =$ The dot product between the surface normal and the vector to the light source.
>Where
> $I_p:$ Intensity of reflected light
>$k_d:$ Diffuse reflectivity coefficirnt
				
				

>[!info] Local Illumination Model V2
>$I =$ ambient + diffuse
>$$I = k_aI_a + I_pK_d (\hat N . \hat L)$$



Ambient versus Point Source

![[Pasted image 20230222192536.png|300]]![[Pasted image 20230222192609.png|300]]

![[Pasted image 20230222192728.png|500]]![[Pasted image 20230222192936.png|500]]

Making diffuse reflection coefficient to 0 makes it purely ambient reflection.

###### Light Source Distance
Light intensity falls off with the square distance travelled, after travelling $d$, the original intensity $I_p$ is now...
$$I_e = \frac{I_p}{4 \pi d^2}$$
Thats correct but we'll introduce an approximation to make it work nicely... The $d^2$ term changes too rapidly, so instead we use:
$$I_e = \frac{I_p}{k_c + k_ld + k_qd^2}$$ and we can tune $k_c$, $k_l$ and $k_q$ for the best results.

>[!info] Local Illumination Model V3
>$I =$ ambient + distance (diffuse)
>$$I = k_aI_a + \frac{I_p}{d'}k_d(\hat N.\hat L)$$ Where $d' = k_c + k_ld + k_qd^2$

### Modelling Specular Reflection

What do we mean?

![[Pasted image 20230222204536.png|200]]
You see the shiny bits of the almond chocolates.. 

![[Pasted image 20230222204713.png|400]]

- $\hat R$ is a vector giving the direction of the maximum specular reflection
- $\hat R$ makes an angle of $\theta$ with $\hat N$, as does $\hat L$
	- note that for a perfect mirror the angle of incidence == the angle of reflection
- $\hat V$ is a vector pointing to the observer's position.

So how much light is reflected to the user?

- Specular reflection varies with:
- Angle $\phi$ between $\hat R$ and $\hat V$
- Incident angle $\theta$ and wavelength $\lambda$
- So we seek a function $I_{\text{specular}} = S(\phi,\ \theta,\ \lambda)$

So how do we model $\phi$?

Say for arguments sake that normalised($\hat V$) = normalised($\hat R$) then:

![[Pasted image 20230222210141.png|400]]
Where $\phi = 0$, what we'll notice, is as we increase $\phi$ we'll notice that the observed specular decreases thus observed specular = $F(\phi)$ but what is $F$? $$F \approx \cos^n\phi$$ in fact.. $F$ is rather complex.. :\*(

![[Pasted image 20230222210640.png|400]]

$$I_{\text{specular}} = I_p\cos^n\phi$$
$$I_{\text{specular}} = I_p(\hat R.\hat V)^n$$ where $1 \leq n \leq 200$

How $n$ affects it..

![[Pasted image 20230222211246.png|400]]

Remember:
$$I_{\text{specular}} = S(\phi,\ \theta,\ \lambda)$$And we've looked at $\phi$, the viewing angle.
The wavelength of illuminant ($\lambda$) and Angle of incidence ($\theta$) are what we gonna look at.

![[Pasted image 20230223110841.png|400]]![[Pasted image 20230223111134.png|400]]
If angle of incidence increases, amount of light reflected also increased (?)

We cannot assume that the amount of reflected light is independent of the angle of incidence but that is just not the case - as showed by the images above... and image below

![[Pasted image 20230223111606.png|400]]

So let's start to define our solution but first let's perform some basic setup:

![[Pasted image 20230223111818.png|300]]

Note that the angle of incidence and reflection are both the same ($\phi$) where some light $\hat L$ upon hitting the surface at the surface normal, is reflected $\hat R$ and some is not transmitted $\hat T$ and has angle $\theta$ from the surface normal.

Fresnel Equation: ![[Pasted image 20230223112259.png|500]]

Where $F$ is the fraction of light reflected, please note that:
$$\sin \theta = \frac{sin \phi}{\mu} $$ where $\mu$ is the refractive index of material with respect to the air - dependent of $\lambda$ 

>[!note] $\theta$ is the angle of transmission, and angle of incidence is $\phi$

$F$ is really complicated to work out for many things so as with all things graphics related we have an approximation. Replace $F$ with a constant $k_s$ where $k_s$ is the specular reflection coefficient of the surface.  $$0 \leq k_s \leq 1$$
$$I_{\text{specular}} - I_pk_s(R.V)^n$$

>[!info] Local Illumination Model V4
>$I$ = ambient + distance (diffuse + specular)
>$$I = k_aI_a + \frac{I_p}{d'}\big[k_d(\hat N. \hat L) + k_s(\hat R. \hat V)^n\big]$$ where $d' = k_c + k_1d + k_qd^2$

![[Pasted image 20230223113301.png|300]]![[Pasted image 20230223113323.png|300]]

![[Pasted image 20230223113501.png|400]]
![[Pasted image 20230223113522.png|400]]
![[Pasted image 20230223113544.png|400]]

What about colour!
- We've only looked at light intensity, well it's easier than you think because we express light as a triple of RGB intensities: $$I_{pR},\ I_{pG},\ I_{pB}$$
And correspondingly we express surface colour using:
$$k_{aR},\ k_{aG},\ k_{aB}$$
$$k_{dR},\ k_{dG},\ k_{dB}$$

>[!info] Local Illumination Model V5
>$I$ = ambient + distance (diffuse + specular)
>$$I_R = k_{aR}I_{aR} + \frac{I_{pR}}{d'}\big[k_{dR}(\hat N. \hat L) + k_{s}(\hat R. \hat V)^n\big]$$ where $d' = k_c + k_1d + k_qd^2$

[This simply implies we have the same model but for each colour R, G and B]

What if we have more than one light? Easy, compute illumination separately for each and sum:

![[Pasted image 20230224090655.png|300]]


>[!question] Phong and Gouraud Shading??

