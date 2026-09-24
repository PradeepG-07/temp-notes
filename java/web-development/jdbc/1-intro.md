## Introduction

JDBC (Java Database Connectivity) is a standard Java API used to interact with relational databases.

There are many database systems available, such as MySQL, PostgreSQL, Oracle Database, and H2. Each database has its own implementation details and communication protocol for establishing connections, executing queries, and returning results or errors.

Since Java cannot provide database-specific implementations for every database vendor, it defines a standard contract through the JDBC API. Database vendors then provide JDBC drivers that implement this contract, allowing Java applications to communicate with their databases in a consistent manner.

The JDBC API is primarily available in the `java.sql` package. Some of the most commonly used types are:

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.Statement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
```

These interfaces and classes are used to establish database connections, execute SQL statements, process query results, and handle database-related exceptions.

## Steps to follow to setup JDBC
1. Import the required packages (required interfaces from java.sql package and the implementation classes from the driver downloaded according to the database which is being used).
2. Load and Register the driver
3. Establish connection
4. Create statement
5. Execute the query
6. Process the result
7. Close the connection

## 1. Loading the Driver
We need to download the JDBC driver respective to the database that we are using. We will be using maven as dependency manager. So we need a depedency `mysql-connector/j`.

Once it is added, maven downloads the dependencies and add it to external libraries.

**Before JDBC 4.0** we need to manually load the JDBC driver using 
`Class.forName()` which forces the JDBC driver class to load and register itself with `DriverManager`.

**Modern JDBC (JDBC 4.0+)** if the JDBC driver JAR is on the classpath, Java automatically discovers and registers the driver using the Service Provider mechanism.

## 2. Establish Connection
To establish a connection in JDBC, we need three parameters. They are:
`url`, `username` and `password`.

### JDBC URL
JDBC URL contains information such as the database type, host, port, database name, and optional connection properties.
```text
jdbc:<database-type>://<host>:<port>/<database-name>

Example:
jdbc:mysql://localhost:3306/localdb
```
| Part      | 	Description                                        |
|-----------|-----------------------------------------------------|
| jdbc      | Indicates that this is a JDBC connection URL.       |
| mysql     | The JDBC driver/database type.                      |
| localhost | 	The hostname where the database server is running. |
| 3306      | 	The port number on which MySQL is listening.       |
| localdb   | 	The database (schema) to connect to.               |

### Connection
`Connection` is an interface from `java.sql` package which holds the connection to the respective database.

To create a connection, we will rely on the another JDBC class called  `DriverManager` and the method `getConnection(url, username, password)`.

## 3. Create Statement

`Statement` is an interface from the `java.sql` package that is used to execute SQL statements against a database. It is created from an active `Connection` object and can be used to execute queries, updates, and other SQL commands.

```java
Statement statement = connection.createStatement();
```

Once created, the `Statement` object can be used to send SQL statements to the database for execution.

## 4. Execute Query
There are three different method provided by `Statement` to execute queries.
1. **executeUpdate**: Used for CREATE, UPDATE, DELETE
2. **executeQuery**: Used for querying SELECT
3. **execute**: Generic purpose

### 4.1 executeUpdate(String sql)
This method will return a `int` to determine if the query is executed successfully or not. Returns `1` if successful or `0` if not successful.

### 4.2 executeQuery(String sql)
This method will return a `ResultSet` which is a way of JDBC to represent the resultant rows.

### ResultSet
ResultSet will maintain a cursor and will be pointed before the first row.
To read the rows one by one, we need to execute `next()` method of the resultset.

To fetch details from a single row, we have two ways
1. **With name:** resultSet.getString("name")
2. **With index:** resultSet.getString(1). **Here index starts with 1**.

### 4.3 execute(String sql)
This method will return a `boolean` to determine if the query result is `ResultSet` or `rowsAffected`.

- If `execute()` returns `true` then the query will be a `SELECT` and data will be stored in `ResultSet` and accessed using `statement.getResultSet()`. 
- If `execute()` returns `false` then the query will be either a `CREATE` or `UPDATE` or `DELETE` and data will be stored in `int` and accessed using `statement.getUpdateCount()`. 

## 5. Process the Result
The result can be processed with either `ResultSet` for select query or `int` for create or update or delete.

### Prepared Statements

`PreparedStatement` is used to execute parameterized SQL queries. Instead of embedding values directly into the SQL string, placeholders (`?`) are used and the values are supplied separately at runtime.

Using `PreparedStatement` provides several benefits:

* Prevents SQL Injection attacks by separating SQL code from user input.
* Improves readability and maintainability of SQL queries.
* Allows the database to reuse execution plans for repeated queries, which can improve performance.

The JDBC API provides the `PreparedStatement` interface in the `java.sql` package. A `PreparedStatement` is created from an active `Connection` object.

```java
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;

Connection connection = DriverManager.getConnection(url, username, password);

String sql = "INSERT INTO table_name VALUES (?, ?, ?)";

PreparedStatement preparedStatement = connection.prepareStatement(sql);
```

Each `?` represents a parameter placeholder. Values can be supplied using type-specific setter methods such as `setString`, `setInt`, `setLong`, `setDouble`, and others.

```java
preparedStatement.setString(1, "John");
preparedStatement.setInt(2, 101);
preparedStatement.setLong(3, 123456789L);
```

The first argument is the parameter index (starting from `1`), and the second argument is the value to be assigned to that placeholder.

After setting all required parameters, the statement can be executed using methods such as `executeQuery()`, `executeUpdate()`, or `execute()`.

## Closing connection
We need to close all the connections that are opened in LIFO order. Code wll be come difficult to read if we use normal try catch block, so it is recommended to use try-with-resources to handle it efficently.

## Transaction
We need manually configure the transaction setup. A snippet for that is below:

```java
import java.sql.DriverManager;

static{ // Ignore this static block
    Connection connection = DriverManager.getConnection("");
    connenction.setAutoCommit(false);
    
    try{
        // first sql query
        // second sql query
        connection.commit();
    }catch (Exception e){
        connection.rollback();
    }finally{
        connection.close();
    }
}
```