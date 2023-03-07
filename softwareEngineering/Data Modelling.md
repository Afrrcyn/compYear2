# Data Modelling

# Data Modelling and Persistence

The core component of an application that is built using the MVC (Model View Controller) pattern is the model. The model is the data and methods that the user is actually using the application for. If there is no data or no model, there is nothing to control or view.

## Modelling data with POJOs

POJO - short for plain old java object

```java
public class Venue {
	private long id;
	
	private String name;

	private int capacity;

	public Venue() {
	}
	
// getters and setters are omitted for brevity
}
```

## Annotate POJOs for Persistence (Saving the data)

```java
@Entity
@Table(name = "venues")
public class Venue {
	private long id;
	
	private String name;

	private int capacity;

	public Venue() {
	}
	
// getters and setters are omitted for brevity
}
```

This is precisely how we persist our venue model in a database

`@Entity` tells Spring that we want this class to be the basis for a database table

`@Table` tells Spring what the table in the database needs to be called

Spring does serialization and deserialization for you

`@id` tells Spring what your primary ID is

`@GeneratedValue` can be used along with `@id` to allow Spring to generate that key for you when the model is saved

# Data Relationships

## Relationship Annotations

`@OneToMany` ”A venue can host many events”

`@ManyToOne` ”An event has one venue”

`@OneToOne` ”A venue has a manager”

> Why is `@ManyToMany` missing
> 

# Data Access Architecture

![Model exists in a database](Data%20Modelling%20d9b229a527b14028b5524c7d9514f50f/Untitled.png)

Model exists in a database

![Untitled](Data%20Modelling%20d9b229a527b14028b5524c7d9514f50f/Untitled%201.png)

Adding a repository layer to hide SQL details from us, and now we can get our data out of the model and into the controller using Java

However, what if we want to process the data before we send it out to the view? Which does make sense putting in account data checks. We can put data processing code in the controller, however that is **bad practice** to do so. Why? Because the model should hold the business logic, and not the controller. We could add it in the repository (Venue repository) but that again is **bad practice** and we should not as the repository talks to the database and we do not want to add complexity in there.

Remember **Separation of Concerns**, and put data processing code elsewhere

<aside>
🙀 What is Separation of concerns?

</aside>

![Untitled](Data%20Modelling%20d9b229a527b14028b5524c7d9514f50f/Untitled%202.png)

We can then divide the Model layer into two parts, where the ‘Service Layer’ can process data for us, and also where the business logic will be

![Untitled](Data%20Modelling%20d9b229a527b14028b5524c7d9514f50f/Untitled%203.png)

Spring makes full use of interfaces to mark different types (object oriented)

As we do not know how Spring has implemented the underlying repository code, we use `@Autowired` annotation and allow Spring to find and initialize the correct one for us

We use an interface for ‘Venue Service’ so that we can control what from the repository is exposed to the controller, and all of this infrastructure is replicated for each type of entity in out model, hence we would see the same sort of things for ‘events’ as well.

# Spring Repositories

```java
public interface VenueRepository extends CrudRepository<Venue, Long> {

}
```

`<Venue, Long>`

`Venue`: Entity type that we want our repository to store

`Long`: Type (we used for the primary key id)

The default implementation provides:

- `count()`
- `findOne(long id)`, `findAll()`
- `save (Venue venue)`, `delete (long id)`
- more…

Spring provides repository implementations. The above example is of a CRUD repository.

`C.R.U.D` = Create, Read, Update, Delete

The above code is all we need to write to tell Spring we want a repository for our venues. This pattern is known as marker (?) interface. This tells Spring to go off and generate a concrete implementation for us automatically

All we have to do is define a marker interface in this way and no other code is required at all, however it is up to us to define some extra methods in this interface

# More Spring Repository Magic

```java
public interface VenueRepository extends CrudRepository<Venue, Long> {
	public Iterable<Venue> findAllByName(String name);
	public Iterable<Venue> findAllByNameOrderByNameAsc(String name);
	public Venue findFirstByNameOrderByNameAsc(String name);
	public Venue findByNameContainingAndCapacity(String nameSearch, int capacity);
	public Iterable<Venue> findAllByCapacityBetween(int min, int max);

}
```

Spring has a mechanism to convert method names into fully implemented queries

# Querying Data via a Service Interface

Create a service interface to expose the methods you need and get Spring to auto-wire the repository

```java
@Service
public class VenueServiceImpl implements VenueService {
	@Autowired
	private VenueRepository venueRepository;
	
	@Override
	public Iterable<Venue> findAll() {
		return venueRepository.findAll();
	}
}
```

We should make sure to split the model layer so the repository only deals with the data

Separation of concerns tells us this functionality should not go into the controller

Also we do not want to expose all of the methods in the repository directly to the controller