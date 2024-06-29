## Composite Primary Keys
A composite primary key, also called a composite key, 
is a combination of two or more columns to form a primary key for a table.
In JPA, we have two options to define the composite keys: the @IdClass and @EmbeddedId annotations.

In order to define the composite primary keys, we should follow some rules:
* The composite primary key class must be public.
* It must have a no-arg constructor.
* It must define the equals() and hashCode() methods.
* It must be Serializable.

### The IdClass Annotation
Let’s say we have a table called Account and it has two columns, accountNumber and accountType, that form the composite key. Now we have to map it in JPA.

As per the JPA specification, let’s create an AccountId class with these primary key fields:

public class AccountId implements Serializable {
private String accountNumber;
```java
private String accountType;

    // default constructor

    public AccountId(String accountNumber, String accountType) {
        this.accountNumber = accountNumber;
        this.accountType = accountType;
    }

    // equals() and hashCode()
}
```

Next let’s associate the AccountId class with the entity Account.

In order to do that, we need to annotate the entity with the @IdClass annotation. We must also declare the fields from the AccountId class in the entity Account and annotate them with @Id:
```java
@Entity
@IdClass(AccountId.class)
public class Account {
@Id
private String accountNumber;

    @Id
    private String accountType;

    // other fields, getters and setters
}
```

### The EmbeddedId Annotation
@EmbeddedId is an alternative to the @IdClass annotation.

Let’s consider another example where we have to persist some information of a Book, with title and language as the primary key fields.
In this case, the primary key class, BookId, must be annotated with @Embeddable:
```java
@Embeddable
public class BookId implements Serializable {
private String title;
private String language;

    // default constructor

    public BookId(String title, String language) {
        this.title = title;
        this.language = language;
    }

    // getters, equals() and hashCode() methods
}
```
Then we need to embed this class in the Book entity using @EmbeddedId:
```java
@Entity
public class Book {
@EmbeddedId
private BookId bookId;

    // constructors, other fields, getters and setters
}
```

### @IdClass vs @EmbeddedId
As we can see, the difference on the surface between these two is that with @IdClass we had to specify the columns twice, once in AccountId and again in Account; however, with @EmbeddedId we didn’t.

There are some other trade offs though.

For example, these different structures affect the JPQL queries that we write.
With @IdClass, the query is a bit simpler:
```SELECT account.accountNumber FROM Account account ```

With @EmbeddedId, we have to do one extra traversal:
```SELECT book.bookId.title FROM Book book```
Also, @IdClass can be quite useful in places where we are using a composite key class that we can’t modify.

If we’re going to access parts of the composite key individually, we can make use of @IdClass, but in places where we frequently use the complete identifier as an object, @EmbeddedId is preferred.

## When Does JPA Set the Primary Key
As we know, JPA (Java Persistence API) uses the EntityManager to manage the lifecycle of an Entity. At some point, the JPA provider needs to assign a value to the primary key. So, we may find ourselves asking, when does this happen? And where is the documentation that states this?
The JPA specification says:
A new entity instance becomes both managed and persistent by invoking the persist method on it or by cascading the persist operation.

### The Generate Value Strategy
When we invoke the EntityManager.persist() method, the entity’s state is changed according to the JPA specification:

If X is a new entity, it becomes managed. The entity X will be entered into the database at or before transaction commit or as a result of the flush operation.
This means there are various ways to generate the primary key. Generally, there are two solutions:

Pre-allocate the primary key
Allocate primary key after persisting in the database
To be more specific, JPA offers four strategies to generate the primary key:
* GenerationType.AUTO
* GenerationType.IDENTITY
* GenerationType.SEQUENCE
* GenerationType.TABLE

### GenerationType.AUTO
AUTO is the default strategy for @GeneratedValue. If we just want to have a primary key, we can use the AUTO strategy. The JPA provider will choose an appropriate strategy for the underlying database:
```java
@Entity
@Table(name = "app_admin")
public class Admin {

    @Id
    @GeneratedValue
    private Long id;

    @Column(name = "admin_name")
    private String name;

    // standard getters and setters
}
```

### GenerationType.IDENTITY
The IDENTITY strategy relies on the database auto-increment column. The database generates the primary key after each insert operation. JPA assigns the primary key value after performing the insert operation or upon transaction commit:
```java
@Entity
@Table(name = "app_user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "user_name")
    private String name;

    // standard getters and setters
}
```
Here, we verify the id values before and after the transaction commit:
```java
@Test
public void givenIdentityStrategy_whenCommitTransction_thenReturnPrimaryKey() {
User user = new User();
user.setName("TestName");

    entityManager.getTransaction().begin();
    entityManager.persist(user);
    Assert.assertNull(user.getId());
    entityManager.getTransaction().commit();

    Long expectPrimaryKey = 1L;
    Assert.assertEquals(expectPrimaryKey, user.getId());
}
```
The IDENTITY strategy is supported by MySQL, SQL Server, PostgreSQL, DB2, Derby, and Sybase.

## GenerationType.SEQUENCE
By using the SEQUENCE strategy, JPA generates the primary key using a database sequence. We first need to create a sequence on the database side before applying this strategy:
```
CREATE SEQUENCE article_seq
MINVALUE 1
START WITH 50
INCREMENT BY 50
```
JPA sets the primary key after we invoke the EntityManager.persist() method and before we commit the transaction.

Let’s define an Article entity with the SEQUENCE strategy:
```java
@Entity
@Table(name = "article")
public class Article {

    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "article_gen")
    @SequenceGenerator(name="article_gen", sequenceName="article_seq")
    private Long id;

    @Column(name = "article_name")
    private String name

    // standard getters and setters
}
```
The sequence starts from 50, so the first id will be the next value, 51.

Now, let’s test the SEQUENCE strategy:
```java
@Test
public void givenSequenceStrategy_whenPersist_thenReturnPrimaryKey() {
Article article = new Article();
article.setName("Test Name");

    entityManager.getTransaction().begin();
    entityManager.persist(article);
    Long expectPrimaryKey = 51L;
    Assert.assertEquals(expectPrimaryKey, article.getId());

    entityManager.getTransaction().commit();
}
```
The SEQUENCE strategy is supported by Oracle, PostgreSQL, and DB2.

### GenerationType.TABLE
The TABLE strategy generates the primary key from a table and works the same regardless of the underlying database.

We need to create a generator table on the database side to generate the primary key. The table should at least have two columns: one column to represent the generator’s name and another to store the primary key value.

Firstly, let’s create a generator table:

@Table(name = "id_gen")
@Entity
public class IdGenerator {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "gen_name")
    private String gen_name;

    @Column(name = "gen_value")
    private Long gen_value;

    // standard getters and setters
}
Copy
Then, we need to insert two initial values to the generator table:

INSERT INTO id_gen (gen_name, gen_val) VALUES ('id_generator', 0);
INSERT INTO id_gen (gen_name, gen_val) VALUES ('task_gen', 10000);
Copy
JPA assigns the primary key values after calling EntityManager.persist() method and before the transaction commit.

Let’s now use the generator table with the TABLE strategy. We can use allocationSize to pre-allocate some primary keys:

@Entity
@Table(name = "task")
public class Task {

    @TableGenerator(name = "id_generator", table = "id_gen", pkColumnName = "gen_name", valueColumnName = "gen_value",
        pkColumnValue="task_gen", initialValue=10000, allocationSize=10)
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE, generator = "id_generator")
    private Long id;

    @Column(name = "name")
    private String name;

    // standard getters and setters
}
Copy
And the id starts from 10,000 after we invoke the persist method:

@Test
public void givenTableStrategy_whenPersist_thenReturnPrimaryKey() {
Task task = new Task();
task.setName("Test Task");

    entityManager.getTransaction().begin();
    entityManager.persist(task);
    Long expectPrimaryKey = 10000L;
    Assert.assertEquals(expectPrimaryKey, task.getId());

    entityManager.getTransaction().commit();
}

---


_References:_
https://www.baeldung.com/jpa-composite-primary-keys
https://www.baeldung.com/jpa-strategies-when-set-primary-key
