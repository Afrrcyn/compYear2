Model-View-Controller is a architectural software design pattern.

Original developed for desktop applications but is now used for web applications a as well.

### Components
Broken down into 3 components (excluding the user of course)

The three components being the view, controller and model. Whereby the user interacts with the view, which updates the controller and the controller then manipulates the model which further updates the view.

To simplify this a step further you can think:
- view = front end (although it's generally thought of as a subset of the data in the model)
- controller = backend
- model = database

Whilst MVC is flexible a common implementation is as such:

Model <=> Controller <=> View

### Any notes from Examinable Reading:

- Models represent knowledge and could be a structure of objects.
- The view is a visual representation of the model, it asks for data from the model and may even update the model.
- Controller is the link between the user and the system. Providing the user with relevant `views`


goto: [[SWE 2 Home Page|Home Page]]
