### What happens internally when we call repository.save() (entity) in Spring Data JPA?

When we call repository.save() in Spring Data JPA, the following steps happen internally.

It calls `entityManager.persist(entity)` if the entity is new (does not have an ID) or `entityManager.merge(entity)` if the entity already exists (has an ID).

The entity is added to the Persistence Context (first level cache).  
Hibernate tracks changes using dirty checking. When the transaction is committed, Hibernate flushes the changes to the database.  
SQL executes during flush(), transaction commit, JPQL query execution.

There is the persist() and merge() method.

persist() - new entities, makes object managed.   
merge() - detached entity, hibernate creates/copies data into a managed instance, returns a different object.

`User managed = em.merge(detachedUser);` The `detachedUser!=managed` it returns a different objects.


### What is the Persistence Context?

A collection of managed entities maintained by EntityManager. The Persistence Context is a first-level cache that stores entities and their state. It ensures that each entity instance is unique within the context, meaning that if you retrieve the same entity multiple times, you will get the same instance. The code seems like `Map<EntityKey,Object>`. 

When we code for the same reference it will return true.



```java
User u1 = em.find(User.class,1);
User u2 = em.find(User.class,1);
System.out.println(u1==u2); // true
```

Hibernate implement dirty checking.