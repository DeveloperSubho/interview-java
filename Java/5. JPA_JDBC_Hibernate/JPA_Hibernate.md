The Java Persistence API (JPA) is a specification of Java. It is used to persist data between Java object and relational database. JPA acts as a bridge between object-oriented domain models and relational database systems.

As JPA is just a specification, it doesn't perform any operation by itself. It requires an implementation. So, ORM tools like Hibernate, TopLink and iBatis implements JPA specifications for data persistence.

**_Let us do discuss some key features of JPA which are as follows:_**

- JPA is only a specification, it is not an implementation.
- It is a set of rules and guidelines to set interfaces for implementing object-relational mapping, .
- It needs a few classes and interfaces.
- It supports simple, cleaner, and assimilated object-relational mapping.
- It supports polymorphism and inheritance.
- Dynamic and named queries can be included in JPA.

The main feature of Hibernate is to map the Java classes to database tables. Following are some key features of Hibernate :

- Hibernate is an implementation of JPA guidelines.
- It helps in mapping Java data types to SQL data types.
- It is the contributor of JPA.

Now you must be wondering out why there is need for JPA, right. So let us discuss the need of JPA before proceeding to differences between them to get cutthroat differences between them. So as illustrated, JPA is a specification. It gives common functionality and prototype to ORM tools. All ORM tools (such as Hibernate) follow the common standards, by executing the same specification. Subsequently, if we need to switch our application from one ORM tool to another then we can easily do it.

By now we have discussed both the concepts and their suitable needs. Now let us jump to the differences between JPA and hibernate and wrap up the article. As we know that JPA is only a specification which means that there is no implementation. We can annotate classes to the extent that we would like with JPA annotations, although, nothing will take place without an implementation. Suppose JPA as the guidelines that should be followed, however, Hibernate is a JPA implementation code that unites the API as described by the JPA specification and gives the anonymous functionality.

Following are the differences between JPA and Hibernate :
|JPA|Hibernate|
|---|---|
|JPA is described in javax.persistence package.|Hibernate is described in org.hibernate package.
|It describes the handling of relational data in Java applications.|
Hibernate is an Object-Relational Mapping (ORM) tool that is used to save the Java objects in the relational database system.|
|It is not an implementation. It is only a Java specification.|Hibernate is an implementation of JPA. Hence, the common standard which is given by JPA is followed by Hibernate.|
|It is a standard API that permits to perform database operations.|It is used in mapping Java data types with SQL data types and database tables.|
|As an object-oriented query language, it uses Java Persistence Query Language (JPQL) to execute database operations.|As an object-oriented query language, it uses Hibernate Query Language (HQL) to execute database operations.|
|To interconnect with the entity manager factory for the persistence unit, it uses EntityManagerFactory interface. Thus, it gives an entity manager.|To create Session instances, it uses SessionFactory interface.|
|To make, read, and remove actions for instances of mapped entity classes, it uses EntityManager interface. This interface interconnects with the persistence condition.|To make, read, and remove actions for instances of mapped entity classes, it uses Session interface. It acts as a runtime interface between a Java application and Hibernate.|

Conclusion: The major difference between Hibernate and JPA is that Hibernate is a framework while JPA is API specifications. Hibernate is the implementation of all the JPA guidelines.
