# Web Frameworks

# Introduction to MVC

MVC (Model - View - Controller) is primarily used to implement user interfaces. Developed for desktop applications but is now commonly used for web applications as well.

MVC provides a complete framework within which to build our software

## MVC Components

1. **Model**: Central component of the pattern. This is the application’s data structure completely independent of the user interface. It contains and manages the data, logic and rules of the application. The model is often implemented as a database with various application specific transformations alongside it.
2. **View**: Represents the data, or a subset of the data, in the model to the user. A view of the data or a view into the data. Often desirable to have multiple representations or views of data
3. **Controller**: Controller accepts input and passes it on as commands to the view or model. Machinery behind the user interface or view, while being independent of them.

![Untitled](Web%20Frameworks%20f70c138077104fe6a3b5bc679f6fdfb1/Untitled.png)

## Putting MVC into Practice

Responses and feedback are fed back to the controller, however this is fine as the MVC is flexible. A good reason to use the controller in this way, in a web implementation of the MVC pattern, is that when the user makes a request they will - either implicitly or explicitly request the response to be in a particular format. For example, some clients can ask for html or XML formats.

The controller, as the interpreter of user actions, has that information so is in, so has a good position to apply a particular view to a response.

![Untitled](Web%20Frameworks%20f70c138077104fe6a3b5bc679f6fdfb1/Untitled%201.png)

So when mapping MVC into the real world, it is worth noting that: 

1. It can be messy
2. Might require some trade-offs
3. More than one way to do it