[[COMP27112]]

### File Formats

The answer to why we have so many file formats is:
- Image Data
- Colour palette
- Different size versions (e.g icons)
- Transparency Information
- Camera data (model, focal length, shutter speed)
- Location (Lat and Long)
And another reason is how we store these properties as well.

##### Image Data
- Grey Scale is 1 byte per pixel
- RGB is 3 bytes per pixel
Would we need to represent a pixel with anything other than these two data types?

When we perform gray-scaling and perform a binary thresholding, pixels become either black or white and so you only need 1 bit per pixel! And so a *Portable Bit Map* formats eight pixels into one byte.

Notice that with PBM files, software can attempt to make grey by having black and white dots, to differentiate between dark and lighter objects. (called *dithering*)

![[Pasted image 20230419202024.png|500]]

### Exposure

How does a camera manage such extreme differences in light?
What does it do with bright rooms and dark rooms?

##### Camera Aperture
- Changes size to allow different amounts of light, we can think of this as the pupil.

##### Camera Shutter
Changing the amount of time that the camera shutter is open also alters the amount of light arriving at the sensor. When you hear a SLR camera click, that is the shutter opening and closing. For a long exposure, the time between the clicks will increase.


![[Pasted image 20230419202912.png|500]]


How does the camera know which one to vary? - Aperture and Shutter. And how do these affect an image other than varying the brightness?

![[Pasted image 20230419203156.png|500]]

SO a slow shutter speed can cause motion blurring.. Fast shutter speed can freeze the action.

![[Pasted image 20230419203352.png|500]]

Imagine it's a windy day and you're taking a photo of a flower blowing in the wind. You'll need a fast shutter (small exposure time) speed to freeze the movement of the flower to allow for the correct amount of light to reach the lens (correct exposure) you need to open the aperture wider.


Small aperture can keep the whole scene in focus, Large Aperture cn reduce the depth of field.. Note that a smaller f equals a larger aperture (aperture = 1/f)

![[Pasted image 20230419203954.png|500]]

##### HDR

Cameras exist where a number of shots are taken in quick succession when you press the shutter button
Each of these images are taken at a different exposure level (called bracketing) and the camera combines them into an ordinary image.
But it would be better to obtain all this information in a single image.

##### Larger Sizes

In most modern CMOS camera sensors are capable of might higher dynamic ranges and in RAW image formats there 12 bits per channel (rather than 8 bits) so they are 4096 levels instead of 256!

Some file formats allow for 16 bits per channel (so 48 bit colour)
- TIFF, PNG and JPEG200 and the HDMI allows for this!

##### Smaller Sizes
What about older file formats? If we have an embedded system with limited memory or display with less colours, can we store images with less data?

![[Pasted image 20230419211930.png|400]]

creates a palette of the most popular colours.. so it allows for the 256 most popular colours and such is diff for each image!

We can also do something like 8 bits for each pixel, 3 bits for R, 3 bits for G and 2 bits for B, this is popular for embedded LCD displays.

##### Multiple Sizes
Windows icon file (.ico) can store multiple different size images in a single file so that the operating system can choose the appropriate one.

##### Transparency

Some file formats allow for transparency with an Alpha channel. You can have a transparency setting like 10% or 50% and not just true or false..

### Compression..

![[Pasted image 20230419213517.png|600]]

Does not work if an image has high frequency (how often a pixel value changes) and you can see this with the bad case.

RLE is also Lossless compression because the original image can be reconstructed. 

![[Pasted image 20230419213831.png|600]]

![[Pasted image 20230419213920.png|600]]
![[Pasted image 20230419214010.png|600]]

each element in the DCT get's divided by the element in the quantisation table. And now we can use RLE: ![[Pasted image 20230419214129.png|600]]
(Note however it's a zig-zag pattern now)

So how do we return the the original image:

![[Pasted image 20230419214229.png|600]]

We have however lost data.. some values remain at zero..

![[Pasted image 20230419214317.png|600]]

![[Pasted image 20230419214410.png|600]]

JPEG is lossy compression, as the image cannot be exactly reconstructed, but the higher quality levels allow for close to the original reconstruction. 









