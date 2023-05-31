[[COMP27112]]
# Introduction

# History

One of the major difficulties with the Cathode Ray tube (CRT) display was Persistence

Persistence: The phenomenon when you light up a region of the display, there’s a phosphorescence on the display which glows for a short period of time, and then fades away to nothing. In simpler terms, if you draw something on a CRT, it fades away rapidly. A solution to this is to refresh the screen very frequently

Set the colour of a conceptual pen, and then move pen to some location. Then draw a line and reset the color to something else, draw a line and then draw some text

These instructions have to be executed very frequently, other wise the whole display will light up with what’s set up and then rapidly fades away.

![Instructions repeat in a loop](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled.png)

Instructions repeat in a loop

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%201.png)

## Raster Graphics Display

Raster graphics uses a 2D array of pixels (screens) or dots (printers), and hence images must be sampled

The more the samples, the better the fidelity (accuracy), however this is always going to be an approximation

You have a block of memory which is mapped to the display. If you need to draw anything to the display, you write to particular pixels in that area of memory and that information appears on the screen.

![The more pixels you have, the better the approximation to the image](Introduction%204500607d0fc64d2a90ceace7e2fbc360/ezgif.com-gif-maker.gif)

The more pixels you have, the better the approximation to the image

<aside>
💡 Fidelity refers to the accuracy or faithfulness with which a representation or recording reproduces the original sound or signal. The statement "the more samples, the better the fidelity" means that the more times the original signal is measured or recorded, the more accurately it can be reconstructed and thus the higher the quality of the reproduction will be. (more like taking mean values to improve a score?)

</aside>

<aside>
💡 Sampling refers to the process of selecting pixels from an image to represent its overall appearance. This is done by reducing the number of pixels in an image, thus reducing its resolution and size, while still preserving its essential features and overall look.

</aside>

### Why Sampling?

Sampling is often done to make images easier to process and store, or to reduce the bandwidth required to transmit images over the internet.

---

<aside>
💡 In Computer Graphics, everything is an approximation. But some approximations are better than others

</aside>

WIMP Interfaces: Windows, icons, menus and pointers

Apple, 1983: Came up with the first graphical user-interface, however IBM came up with a PC that had slightly higher resolution screen, less RAM

# OpenGL

We are expected to use three.js which is a wraparound for WebGL, which in turn is a web-based version of OpenGL

Open Graphics Library

- Platform/language-independent
- Origins in GL, by Silicon Graphics, Inc.
- First released 1992
- Today, open standard for portable graphics programming
- www.opengl.org

## OpenGL vs. DirectX

OpenGL

- Open standard, run by **Khronos Group**
- Cross-architecture (Win/Mac/Linux)

DirectX

- Owned by Microsoft

Functionally, the two systems are roughly the same, however OpenGL is open software whereas DirectX only runs on Microsoft hardware

### Where does OpenGL fit in

If you want to create a rendering of some model, you will start off with an application model, a model of some data that you want to render (or draw) a picture of that and that will be fed into an application program which will take the model and it will decide how to draw that on the screen. All of this is converted into a sequence of commands for the OpenGL API. The API then drives the display and users will then see something on screen.

The library also provides a way of receiving input from various devices. And those inputs will also pass through the OpenGL API back into your program, and maybe used to update the model and therefore change what appears on the screen.

### The OpenGL API

OpenGL is a specification of an Application Programmer’s Interface (API), so language in a set of functions for doing 3D computer graphics. This is used in mobile devices, industry, engineering, CAD, CGI, FX, research, games - everywhere

## Portability

The API that you write the program in will interface to the OpenGL library, and then when you compile the whole caboodle (grouped), we end up with some code which could run on a portable device on any platform

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%202.png)

Desktop version of OpenGL is different to the portable version of OpenGL

### OpenGL Evolution

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%203.png)

## Basic Graphics System Architecture

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%204.png)

Application program uses a graphic software that incorporates some functions for drawing basic shapes to build up objects. Transformations of those basic shapes will include methods of working out what those shapes or what that accumulation of shapes looks like from some particular viewing direction. The output of the graphics software is a set of pixels which his written into frame buffer memory. That frame buffer memory maps onto the screen memory (what does this mean?). All graphics systems work the same way. Application program is run on your CPU whereas graphic software often runs on a graphics processing unit (GPU), because although the operations are simple, they occur a lot of times, hence your GPU would speed up the rendering process.

<aside>
🎭 Render / Rendering: Rendering in computer graphics refers to the process of generating a photorealistic or non-photorealistic 2D or 3D image from a model by simulating the interactions of light with the virtual objects in a scene. This includes shading, shadows, reflections, and other visual effects, and can be done using a variety of algorithms, including rasterization and ray tracing. The result of this process is a final visual representation of the scene that can be displayed on a screen, stored in a file, or printed.

</aside>

## Graphics Pipeline

### Fixed Functionality

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%205.png)

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%206.png)

Input of 3D vertices and output of pixels. Sequence of operations is fixed, there will always be these sequences in any particular application. The API can however change the parameters that are input to each of these operations.

### Programmable Functionality

The pipeline is now a mixture of fixed and programmable

Some sequences are still fixed however there are two functions which the programmer has to implement himself. 

Shaders:

1. Vertex Shader Program (runs per-vertex)
    
    Decides the location of each vertex in the model
    
2. Fragment Shader Program (runs per-fragment)
    
    Shades in the surfaces that are being dealt with
    

<aside>
🎭 These two shaders must be provided by the user, written in a shading language (e.g. GLSL). The user has access to state in the system.

</aside>

![This is the basic structure. There are other kinds of user-supplied programs too, e.g. ‘geometry’ and ‘tessellation’ shaders|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%207.png)

This is the basic structure. There are other kinds of user-supplied programs too, e.g. ‘geometry’ and ‘tessellation’ shaders

### An example GLSL Vertex Shader

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%208.png)

### An example GLSL Fragment Shader

![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%209.png)

## Fixed versus Programmable Pipelines

Fixed Pipeline:

- con: new algorithms and techniques cannot be added
- con: it’s deprecated
- pro: simple to use and fine for many purposes
- pro: for the beginner it’s easy to get started quickly

Programmable Pipeline:

- con: for the beginner there is significant start-up
- pro: provides maximum flexibility
- pro: state-of-the-art, cutting edge, new stuff all the time

## OpenGL and Interaction

- OpenGL only generates pixels
- It does not handle interaction devices
- For interaction, we use the `GLUT` library, however there are other GUI libraries

![Untitled|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2010.png|)

### Graphics

The main features of OpenGL are:

- 3D graphics (points, lines, polygons…)
- Co-ordinate transformations
- Camera (for viewing)
- Hidden surface removal
    
    ![GIF|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled.gif)
    
    GIF
    
- Lighting and shading
- Texturing
- Modelling and rendering
    
    ![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2011.png)
    
- Pixel (image) operations
- Blending / transparency
- Points
    
    ![Untitled](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2012.png)
    
- Lines
    
    ![Untitled|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2013.png)
    
- Polygons
    
    ![Untitled|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2014.png)
    

### Transformations

- Scaling
    
    ![Untitled|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2015.png)
    

- Shearing
    
    ![Untitled|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2016.png)
    

- Rotations
    
    ![Untitled|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2017.png)
    

### Support Libraries

![Untitled|400](Introduction%204500607d0fc64d2a90ceace7e2fbc360/Untitled%2018.png)

## GLU

GL Utility Library (GLU)

Provides functions which ‘wrap up’ lower-level OpenGL graphics

- Curves, cylinders, surfaces, disc, quadrics

Utility functions:

- Viewing
- Textures
- Tessellation

## GLUT

GL Utility Toolkit *GLUT)

Interaction

- Interaction (mouse and keyboard)
- Simple Menu system

Primitives

- Sphere, torus, cone, cube
- [tetra | octo | …]hedron
- Teapot