**1. What Is Spring JdbcTemplate Class and How to Use It?**
The Spring JDBC template is the primary API through which we can access database operations logic that we’re interested in:

- Creation and closing of connections
- Executing statements and stored procedure calls
- Iterating over the ResultSet and returning results
  In order to use it, we'll need to define the simple configuration of DataSource:

```
@Configuration
@ComponentScan("org.baeldung.jdbc")
public class SpringJdbcConfig {
    @Bean
    public DataSource mysqlDataSource() {
        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/springjdbc");
        dataSource.setUsername("guest_user");
        dataSource.setPassword("guest_password");

        return dataSource;
    }
}
```

**2. How to Enable Transactions in Spring and What Are Their Benefits?**
There are two distinct ways to configure Transactions — with annotations or by using Aspect-Oriented Programming (AOP) — each with their advantages.

Here are the benefits of using Spring Transactions, according to the official docs:

- Provide a consistent programming model across different transaction APIs such as JTA, JDBC, Hibernate, JPA and JDO
- Support declarative transaction management
- Provide a simpler API for programmatic transaction management than some complex transaction APIs such as JTA
- Integrate very well with Spring's various data access abstractions

**3. What Is Spring DAO?**
Spring Data Access Object (DAO) is Spring's support provided to work with data access technologies like JDBC, Hibernate and JPA in a consistent and easy way.
