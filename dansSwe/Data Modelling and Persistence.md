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
```

Please note that getters and setters have to be named appropriately for Spring to recognise.

The `@Entity` annotation here tells Spring to make a table in the DB for these entities - please note that the `Table(...)` annotation is completely optional here!

Spring also allows for relationships between Entities
`@OneToMany`, `@ManyToOne` and `@OneToOne`

Below is the Data Access Sequence:
Database \<sql\> Repository <=> Service <=> Controller \<http> View

Repositories will `extend CrudRepository<Object, Long>` where Long is the type of the primary key. This will enable Spring to automatically make all functions defined in the file.