1. What is JDBC Driver?
   JDBC Driver is a software component that enables java application to interact with the database. There are 4 types of JDBC drivers:
1. JDBC-ODBC bridge driver
1. Native-API driver (partially java driver)
1. Network Protocol driver (fully java driver)
1. Thin driver (fully java driver)
1. What are the steps to connect to a database in java?
   • Registering the driver class
   • Creating connection
   • Creating statement
   • Executing queries
   • Closing connection
1. What are the JDBC API components?
   The java.sql package contains interfaces and classes for JDBC API.
   Interfaces:
   • Connection
   • Statement
   • PreparedStatement
   • ResultSet
   • ResultSetMetaData
   • DatabaseMetaData
   • CallableStatement etc.
   Classes:
   • DriverManager
   • Blob
   • Clob
   • Types
   • SQLException etc.
1. What is the role of JDBC DriverManager class?
   The DriverManager class manages the registered drivers. It can be used to register and unregister drivers. It provides factory method that returns the instance of Connection.
1. What is JDBC Connection interface?
   The Connection interface maintains a session with the database. It can be used for transaction management. It provides factory methods that returns the instance of Statement, PreparedStatement, CallableStatement and DatabaseMetaData.

In case you are facing any challenges with these java interview questions, please comment on your problems in the section below. 6. What is the purpose of JDBC ResultSet interface?
The ResultSet object represents a row of a table. It can be used to change the cursor pointer and get the information from the database. 7. What is JDBC ResultSetMetaData interface?
The ResultSetMetaData interface returns the information of table such as total number of columns, column name, column type etc. 8. What is JDBC DatabaseMetaData interface?
The DatabaseMetaData interface returns the information of the database such as username, driver name, driver version, number of tables, number of views etc. 9. What do you mean by batch processing in JDBC?
Batch processing helps you to group related SQL statements into a batch and execute them instead of executing a single query. By using batch processing technique in JDBC, you can execute multiple queries which makes the performance faster. 10. What is the difference between execute, executeQuery, executeUpdate?
Statement execute(String query) is used to execute any SQL query and it returns TRUE if the result is an ResultSet such as running Select queries. The output is FALSE when there is no ResultSet object such as running Insert or Update queries. We can use getResultSet() to get the ResultSet and getUpdateCount() method to retrieve the update count.
Statement executeQuery(String query) is used to execute Select queries and returns the ResultSet. ResultSet returned is never null even if there are no records matching the query. When executing select queries we should use executeQuery method so that if someone tries to execute insert/update statement it will throw java.sql.SQLException with message “executeQuery method can not be used for update”.
Statement executeUpdate(String query) is used to execute Insert/Update/Delete (DML) statements or DDL statements that returns nothing. The output is int and equals to the row count for SQL Data Manipulation Language (DML) statements. For DDL statements, the output is 0.
You should use execute() method only when you are not sure about the type of statement else use executeQuery or executeUpdate method.
Q11. What do you understand by JDBC Statements?
JDBC statements are basically the statements which are used to send SQL commands to the database and retrieve data back from the database. Various methods like execute(), executeUpdate(), executeQuery, etc. are provided by JDBC to interact with the database.
JDBC supports 3 types of statements:

1. Statement: Used for general purpose access to the database and executes a static SQL query at runtime.
2. PreparedStatement: Used to provide input parameters to the query during execution.
3. CallableStatement: Used to access the database stored procedures and helps in accepting runtime parameters.
