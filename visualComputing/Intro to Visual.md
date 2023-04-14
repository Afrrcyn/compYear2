[[COMP27112]]

Image Processing - the manipulation or modification of a digitised image especially in order to enhance it's quality.

### Image Representation:

Resolution:
- 12 Megapixel (12 Million Pixels)
- $4272 \times 2848$ pixels
- Allows for zoom whilst zooming in.

Colour Depth
- 24-bit
- $2^{24} \approx 16, 000,000$

RGB Format
- Red, Green and Blue channels
- One byte per colour and Three bytes per pixel.

Greyscale
- One byte per pixel
- Values between $0$ and $255$

Image coordinates use the top-left as the origin, which is not the most common usage. We store images in a computer as a matrix. So it's information is stored (rows, columns) *Rhubarb and Custard*

For every pixel $(x, y)$ in an image, we can find the intensity using the function $I(x, y)$ (and remember the intensity goes from $0$ to $255$)

SO thresholding how does this work?

$$I'(x, y) = \begin{cases} 255 & \text{if } I(x, y) \geq T \\ 0 & \text{otherwise}\end{cases}, \text{where } T \in [0, 255]$$

### OpenCV

Originally developed by Intel, it's now an Open Source Computer Vision/ Image Processing library.

![[Pasted image 20230307082250.png|400]]

`Mat` refers to matrix and remember this is how we store images.

![[Pasted image 20230307090253.png|400]]


![[Pasted image 20230307090712.png|600]]

In $1665$ Sir Isaac Newton discovered that light is composed of Red, Orange, Yellow, Green, Blue, Indigo and Violet -  we know there are for more.

Thomas Young was looking at light and discovered you can approximate most colours with just Red Green and Blue (not all colours ofcourse)

In your eyes you have 
- Rods - for low light vision - no colour
- Cones - for colour

There are three types of cones:
- S - sensitive to short-wavelengths visible light (roughly blue)
- M - sensitive to medium-wavelength light (roughly green)
- L - sensitive to long-wavelength light (roughy red)

65% of cones are sensitive to Red, 33% for green and 2% for blue (but they are the most sensitive)

![[Pasted image 20230307092918.png|500]]

![[Pasted image 20230307092959.png|500]]

[Note the one on the left is for light!]

White light from the sun illuminates on a leaf, the pigments in the leaf reflects only green and absorbs all other colours and the reflected green light hits our cones and we see leaves as green.

For screens we need some sort of standardisation in terms of the colours that the screen displays:

So the CIE 1931 XYZ colour space was chosen:

![[Pasted image 20230307093500.png|400]]

Note these actually don't line up with our cones simply because the data from the cone experiment was not collected yet -  in 1931.

![[Pasted image 20230307093719.png|500]]

![[Pasted image 20230307093852.png|400]]

The colour section is what's visible by the human eye. And the triangle shows what can be represented with the CIE standard.

And there are many standards for colours:
- YCrCb - used for broadcasting
- HSV, IHS, HSB

The RGB colour space can actually be represented using a cube (0, 0, 0) -> (255, 255, 255)

Let's look at the YCrCb standard in more detail:

![[Pasted image 20230307094211.png|500]]

Why perceptual spaces?
- equal distances on CIE diagram DO NOT correspond to equal changes in perceived colour
- This is important for measurement and allow colours to be described in a suitable way for humans.

ISH (or HSI) Model

Intensity:
- Brightness
- Weighted Average of the RGB

Hue
- The basic colours
- An angle from 0 to 359

Saturation
- Depth of colour
- (lower saturation means more diluted with white)

![[Pasted image 20230307095907.png|600]]

![[Pasted image 20230307100643.png|500]]

Another colour Space is called Lab
or (L\*a\*b*) and CIELAB
- Used in photoshop
- Device independent
- L* represents lightness
	- 0 = black, 100 = white
- a* represents green to red
	- -a = green, +a = red
- b* represents blue to yellow
	- -b = blue, +b = yellow

![[Pasted image 20230307100919.png|400]]

Why do standards only cover a subset of the visible colours that humans can see?

![[Pasted image 20230307101158.png|600]]

OpenCV stores images in BGR, and we can convert colours like the code above shows (line 18)

![[Pasted image 20230307101335.png|500]]

So using this information can allow us to perform thresholding and find all yellow objects within a photo:

![[Pasted image 20230307101457.png|600]]

When taking a greyscale, we we take the average of the RGB components:

![[Pasted image 20230307101714.png|400]]

But we want some sort of difference... So we should use some sort of weighted average to get a difference shading so we can differentiate between all 3 colours.