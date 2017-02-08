---
title: spring
date: 2017-02-08 13:58:24
tags:
---

## JPA

JPA Specification : hibernate, openjpa, toplink and so on.

Config jpa in spring by defining two beans, datesource and entityManagerFactory.

Entity reflect database foreign keys and relationship in class attributes.

Check 
[OneToMany docs](http://docs.oracle.com/javaee/7/api/javax/persistence/OneToMany.html).


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

