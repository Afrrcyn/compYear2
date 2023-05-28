POJO - Plain Old Java Object

We will need to annotate our POJOs for persistence:

```java

@Entity
@Table(name = "objects")
public class Object {
	private long id;
	private String name;
	private int amount;
	
	getId() {return id;}
	setId(id) {this.id = id;}
	
}

/*
@id: tells Spring what your primary ID is
@GeneratedValue can be used along with @id to allow Spring to generate that key for you when the model is saved
*/
```

Please note that getters and setters have to be named appropriately for Spring to recognise.

The `@Entity` annotation here tells Spring to make a table in the DB for these entities - please note that the `Table(...)` annotation is completely optional here!

Spring does serialisation (converting Java objects into a byte stream) and deserialisation (converting the byte stream back into Java objects) for you
Spring also allows for relationships between Entities
`@OneToMany`(A venue can host many events), `@ManyToOne`(Three events have one venue) and `@OneToOne`(One venue has one manager)

Below is the Data Access Sequence:
Database \<sql\> Repository <=> Service <=> Controller \<http> View

Repositories will `extend CrudRepository<Object, Long>` where Long is the type of the primary key. This will enable Spring to automatically make all functions defined in the file.

#### Data Access Architecture
![[Pasted image 20230528210813.png|300]]
Adding a repository layer to hide SQL details from us, and now we can get our data out of the model and into the controller using Java.
Then, in order to process the data before sending it to the view, we use **Separation of Concerns** to put data processing elsewhere because we can't put it in the database repository as we want our repository to just talk to the database and not bring complexities.
![[Pasted image 20230528211029.png|400]]
Here, the 'Service Layer' will process the data for us
![[Pasted image 20230528211122.png|400]]
As we do not know how Spring has implemented the underlying repository code, we use `@Autowired` annotation and allow Spring to find and initialise the correct one for us

We use an interface for ‘Venue Service’ so that we can control what from the repository is exposed to the controller, and all of this infrastructure is replicated for each type of entity in out model, hence we would see the same sort of things for ‘events’ as well.

>[!info] `@Autowired`
>It allows Spring to automatically find the appropriate dependency and inject it into the annotated field, constructor, or method.

![[Pasted image 20230528211558.png|500]]


![[Pasted image 20230528211510.png|500]]

