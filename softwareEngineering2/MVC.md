Model-View-Controller is an architectural software design pattern.

Originally developed for desktop applications (to implement user interfaces) but is now used for web applications as well.

### Components
Broken down into 3 components (excluding the user of course)

The three components being the model, view and controller. Whereby the user interacts with the view, which updates the controller and the controller then manipulates the model which further updates the view.
![[Pasted image 20230527225820.png|300]]

To simplify this a step further you can think:
- view = front end (although it's generally thought of as a subset of the data in the model)
- controller = backend
- model = database

##### Model
Models represent knowledge.
**[[Application]]**'s data structure which is completely independent of the user interface. Contains and manages the data, logic and rules of the application. Is often implemented as a database

##### View
A visual representation of it's model.
Represents the data or a subset of the data in the model to the user. Desirable to have multiple views or representations of data

##### Controller
Link between a user and the system. A controller should never connect the views of nodes by drawing arrows between them.
Accepts input and passes it on as commands to the view or model.

Whilst MVC is flexible a common implementation is as such:

Model <=> Controller <=> View

As the controller can be imagined as the backend, responses and feedback are fed back into the controller as when a user makes a request, they will either implicitly or explicitly request the response to be in a particular format.

![[Pasted image 20230527232747.png|400]]

### Any notes from Examinable Reading:

- Models represent knowledge and could be a structure of objects.
- The view is a visual representation of the model, it asks for data from the model and may even update the model.
- Controller is the link between the user and the system. Providing the user with relevant `views`
- Some views provide a special controller, an ==editor== that permits the user to modify information that is presented by the view. It acts as an extension of the controller. Once the editing part is done, the editor is removed from the path and discarded.
- The editor is closely associated with the view. A controller will get hold of an editor by asking the view for it.


goto: [[SWE 2 Home Page|Home Page]]
