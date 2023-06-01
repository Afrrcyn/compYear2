[[COMP27112]]

1920
Bartlane Picture Transmission Service
- used submarine telegraph cables to send pictures across the Atlantic.
- They can reproduce the image by decoding the grey dots

1964
Computer Enhancement of Images from NASA Moon Probes
- The camera in the probe was not the best and images sent back were distorted so they had to fix that.

1979
Computer Assisted Tomography
- CT scans

Anything that can generate a spatially coherent measurement of some property can be imaged:
- Can be energy sources
	- EM waves
		- gamma - Ingest it!
		- x-rays - obvious init..
		- visible - LIDAR
		- microwave
		- radio - RADAR
	- Others
		- Sound - Ultrasound and Sonar (similar to RADAR and LIDAR but for the ocean)
		- Magnetic

common application areas
- Medicine - Scans (CT, Ultrasound)
- Oil Exploration - reflection seismology to see whats under the ground.
- Astronomy - Taking pics of Galaxies
- Weather
- Agriculture - Auto-Pruning
- Policing and Security - Number Plates Recognition, Footprint detection 
- Satellite Imagery
- Archeology
- Beauty Industry (Snapchat)

### Spatial Resolution

##### Angular Resolution

![[Pasted image 20230313084335.png|500]]

We can see an example with Aerial Photography:

![[Pasted image 20230313084514.png|500]]

And we would like to know the ground resolution as then we can know the detail of what we detected and whether that amount of detail is sufficient.

![[Pasted image 20230313084648.png|500]]

So here a car may take up 2-3 pixels - is that the amount of detail you want? A person will take up 1 pixel...

>[!info] Nyquit's Theorem
>- A periodic signal can be reconstructed if the sampling interval is at least half the period
>- An object can be detected if two samples span it's smallest dimension
>- More samples are needed if we want to recognise the object

##### Intensity Resolution
- Humans can see about
	- 40 shades of brightness
	- 7.5 million shades of colour
- Images are commonly stored with:
	- 256 shades of grey (brightness)
	- 16.8 million shades of colour

![[Pasted image 20230313085226.png|500]]
[ignore the damaged right side of the right image]
![[Pasted image 20230313085300.png|500]]
We still lose detail by decreasing the amount of different shades for brightness


##### Noise
An image taken in a well-lit room with have little sign of noise, when taken in a dark room that's when we get noise - vertical and grainy lines

![[Pasted image 20230313085504.png|500]]

Even when you take a black image, you'll still get noise.

![[Pasted image 20230313085611.png|500]]

- Here we have a grey rectangle with each value set to 128
- Gaussian noise has been added now, with a Mean of 0 and STD of 8.
- To find the signal to noise ratio
	- SNR = $20 \log_{10}(255/8) = 30$dB
	- Higher dB = Lower Noise
	- [255 is the range of values and 8 is the STD]
>[!Note]
>Higher the dB, lower the noise (do not confuse noise with sound)

