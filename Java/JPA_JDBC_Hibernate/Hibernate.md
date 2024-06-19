Hibernate is used to overcome the limitations of JDBC like:

1. JDBC code is dependent upon the Database software being used i.e. our persistence logic is dependent, because of using JDBC. Here we are inserting a record into Employee table but our query is Database software-dependent i.e. Here we are using MySQL. But if we change our Database then this query won’t work.
   ![JDBC](/Screenshots/jdbc.jpg)

2. If working with JDBC, changing of Database in middle of the project is very costly.
3. JDBC code is not portable code across the multiple database software.
4. In JDBC, Exception handling is mandatory. Here We can see that we are handling lots of Exception for connection.
   ![JDBC](/Screenshots/jdbc_exception.jpg)

5. While working with JDBC, There is no support Object-level relationship.
6. In JDBC, there occurs a Boilerplate problem i.e. For each and every project we have to write the below code. That increases the code length and reduce the readability.
   ![JDBC](/Screenshots/jdbc_boilerplate.jpg)

To overcome the above problems we use ORM tool i.e. nothing but Hibernate framework. By using Hibernate we can avoid all the above problems and we can enjoy some additional set of functionalities.

**_Functionalities supported by Hibernate framework_**

- Hibernate framework support Auto DDL operations. In JDBC manually we have to create table and declare the data-type for each and every column. But Hibernate can do DDL operations for you internally like creation of table,drop a table,alter a table etc.
- Hibernate supports Auto Primary key generation. It means in JDBC we have to manually set a primary key for a table. But Hibernate can this task for you.
- Hibernate framework is independent of Database because it supports HQL (Hibernate Query Language) which is not specific to any database, whereas JDBC is database dependent.
  In Hibernate, Exception Handling is not mandatory, whereas In JDBC exception handling is mandatory.
- Hibernate supports Cache Memory whereas JDBC does not support cache memory.
- Hibernate is a ORM tool means it support Object relational mapping. Whereas JDBC is not object oriented moreover we are dealing with values means primitive data. In hibernate each record is represented as a Object but in JDBC each record is nothing but a data which is nothing but primitive values.

_References:_
https://www.geeksforgeeks.org/introduction-to-hibernate-framework/
https://www.edureka.co/blog/what-is-hibernate-in-java/
https://howtodoinjava.com/series/hibernate-tutorials/
