---
title: spring
date: 2017-02-08 13:58:24
tags:
---
## Spring doc
[Spring docs](http://docs.spring.io/spring-boot/docs/current/reference/html/index.html).

## JPA

JPA Specification : hibernate, openjpa, toplink and so on.

Config jpa in spring by defining two beans, datesource and entityManagerFactory.

Entity reflect database foreign keys and relationship in class attributes.

Check 
[OneToMany docs](http://docs.oracle.com/javaee/7/api/javax/persistence/OneToMany.html).

Keywords: 

Role,Direction(unidirectional,bidirectional),Cardinality,Ordinality.
Owner Entity, Non-owner Entity, Source, Target.

```
@Entity
@Table(name="user")
public class User implements Serializable{
	@OneToMany
	private Set<Transaction> transactions = new LinkedHashSet<Transaction>();
}
```

Specifying Many as the second part declares that the origin entity (User) can have several target Entities (Transactions). These targets will have to be wrapped in a collection type. If the origin entity cannot have several targets, then the second part of the relationship has to be One.

A Transaction can have only one User entity.

We can then deduce the two annotations: @OneToMany in User and @ManyToOne in Transactions.

<!-- more -->
JoinColumns are nothing but a Foreign Key Columns.
JPA calls them Join Columns, possibly because they are more verbose in what their actual role is, to join the two entities using a common column.

```
@Entity
public class Employee implements Serializable{
	@ManyToOne
	private Department department;
}

/**or**/
/**ManyToOne mapping are defined on the owner entity.**/
@Entity
public class Employee implements Serializable{
	@Id private int id;

	@ManyToOne
	@JoinColumn(name="DEPT_ID")
	private Department department;
	//otherwise, default name will be department_id
}
```

### Bidirectional OneToOne Mapping.

We know that the owner always have a JoinColumn annotation. In the case of OneToOne mapping, any entity can be the owner. The other non-owner entity then has to provide the mappedBy attribute value to indicate that it is not the owner of the relationship. 

```
@Entity
public class Job {
    @Id private int id;
    private String designation;
    private String location;
    @OneToOne(mappedBy="job")
    private Person person;
    //the mappedBy attribute refers to the attribute name job, defined in the source entity Person.
}
```

### OneToMany

```
@Entity
public class Book {
    @Id private int id;
    @OneToMany
    @JoinColumn(name="BOOK_ID") // BOOK_ID exists in the BookMark table
    private List bookMarks;
    // ... 
}
```

### Bidirectional OneToMany Mapping

When the relationship is bidirectional, there are normally two associations defined : 
one from source to target entity(having the JoinColumn annotation) , 
and the other from target to source entity(having the mapped by attribute ). 
OneToMany bidirectional mapping always implies a ManyToOne mapping back to the source. 
Another important thing to remember from the previous discussion about ManyToOne mapping is that if theres is a ManyToOne mapping, then the JoinColumn annotation will always be defined on the ManyToOne mapping attribute. Thus in case of Bidirectional OneToMany mapping, the JoinColumn will exists on the entity having ManyToOne annotation. 


### Bidirectional ManyToMany Mapping

We do that by defining the mappedBy attribute of the relationship on the target entity. 
Note that since the multiplicity of both the sides of the relationship is plural, it is not possible(or feasible/correct) to store an iunlimited set of foreign key values in a single entity row. 
We should use the third table to associate the two entities. 
This third table is called the ~~JoinTable~~. 
A Join Table is a requirement for a ManyToMany relationship. 
A JoinTable consists of the Primary Keys from both the entities and nothing else. Here is an example of ManyToMany relationship from Pro JPA2 book.

![](/images/ManyToMany.png)

In the above example, the Employee and Project table are two entities related with ManyToMany relationship. EMP_PROJ table is the JoinTable that is used to store the entity reference for each of the table.
In JPA, we use the @JoinTable annotation to define a Join Table for the Many to Many relationship. Here is an example

```java
@Entity
public class Employee {
    @Id private int id;
    private String name;
    @ManyToMany
    @JoinTable(name="EMP_PROJ",
          joinColumns=@JoinColumn(name="EMP_ID"),
          inverseJoinColumns=@JoinColumn(name="PROJ_ID"))
    private Collection projects;
    // ...
}
```

* The JoinTable annotation has a name attribute which corresponds to the name of the Join Table. It also has two other attributes :

- joinColumns which is the column name of the owning Entity
+ inverseJoinColumns which is the column name of the inverse or non owning Entity. 


## JPA Criteria

to build query programmatically, define where clause of a query for a domain class.

Eric Evans "Domain Driven Design"

* [SpecificationArgumentResolver with Filter Spec](http://blog.kaczmarzyk.net/2014/03/23/alternative-api-for-filtering-data-with-spring-mvc-and-spring-data/).

* [Easy Pagination with Spring Boot](http://ankushs92.github.io/tutorial/2016/05/03/pagination-with-spring-boot.html).


## Exception Handler

* [@ControllerAdvice](https://spring.io/blog/2013/11/01/exception-handling-in-spring-mvc)
* [Demo of Exception](http://mvc-exceptions-v2.cfapps.io/)


## CommandLineRunner#init

```java 
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
  CommandLineRunner init(CustomerRespository repo) {
  return (evt) ->  {
    repo.save(new Customer("Adam","adam@boot.com"));
    repo.save(new Customer("John","john@boot.com"));
    repo.save(new Customer("Smith","smith@boot.com"));
    repo.save(new Customer("Edgar","edgar@boot.com"));
    repo.save(new Customer("Martin","martin@boot.com"));
    repo.save(new Customer("Tom","tom@boot.com"));
    repo.save(new Customer("Sean","sean@boot.com"));
  };
  }
}
```

## Actuators monitor and manage boot application


## Richardson's maturity model

+ level 0 : http
+ level 1 : resources
+ level 2 : http verbs
+ level 3 : hypermedia controls (make it discoverable through the use of hyperlinks, like HATEOAS)


## view resolver

### render

[Spring MVC](http://docs.spring.io/spring-boot/docs/current/reference/html/howto-spring-mvc.html).

## Logging

[Spring Logging](http://docs.spring.io/spring-boot/docs/current/reference/html/howto-logging.html).


## Events

[Spring Events](http://zoltanaltfatter.com/2016/05/11/application-events-with-spring/).
[Spring Events](https://www.keyup.eu/en/blog/101-synchronous-and-asynchronous-spring-events-in-one-application).


## run in community intellij

we can do it with embedded tomcat

mvn tomcat7:run

```xml
<plugin>
    <groupId>org.apache.tomcat.maven</groupId>
    <artifactId>tomcat7-maven-plugin</artifactId>
    <version>2.2</version>
    <configuration>
        <path>/</path>
    </configuration>
</plugin>
```

## Spring Server-Sent Events

opening an HTTP connection for receiving push notifications from a server in the form of DOM events.





