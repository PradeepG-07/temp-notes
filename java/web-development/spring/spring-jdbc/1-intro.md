# Spring JDBC

Spring JDBC is an abstraction layer built on top of JDBC. While JDBC is powerful, it requires developers to repeatedly perform several boilerplate operations such as:

1. Obtaining a database connection
2. Creating a `Statement` or `PreparedStatement`
3. Executing the SQL statement
4. Processing exceptions
5. Closing the statement and connection

Spring JDBC removes this repetitive code and allows developers to focus only on the parts that change:

1. SQL queries
2. Parameter values
3. Result mapping

Spring achieves this through the **Template Method Design Pattern**.

The Template Method pattern is commonly used when a sequence of operations must always be executed in a fixed order, while some steps may vary depending on the use case.

For example, suppose four methods `m1()`, `m2()`, `m3()`, and `m4()` must always execute in the following order:

```text
m1 → m2 → m3 → m4
```

The implementation of `m1()` and `m3()` remains constant, while `m2()` and `m4()` may vary. In such cases, a template method can define the overall workflow while delegating the varying parts to subclasses or callback implementations.

Spring JDBC follows a similar approach through the `JdbcTemplate` class.

`JdbcTemplate` manages the common JDBC workflow:

* Obtaining a connection
* Creating statements
* Executing SQL
* Handling exceptions
* Closing resources

The developer only provides:

* SQL queries
* Query parameters
* Row mapping logic

As a result, the application code becomes significantly simpler and easier to maintain.

---

## Connection and DataSource

A `DataSource` is an interface responsible for creating and managing database connections. It serves a similar purpose to JDBC's `DriverManager`, but provides additional features such as connection pooling.

Database connection properties are typically configured in `application.properties` or `application.yaml`:

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/localdb
spring.datasource.username=root
spring.datasource.password=password
```

Spring Boot reads these properties and automatically creates a configured `DataSource`.

### Why Connections Are Expensive

Creating a new database connection is an expensive operation because it involves several steps:

1. Establishing a network connection to the database server
2. Authenticating with the provided credentials
3. Creating a database session
4. Allocating server-side resources

If a new connection were created for every request, the database would quickly become overloaded. Since every database has a limited capacity to handle concurrent connections, continuously opening and closing connections is inefficient.

To solve this problem, applications use **connection pooling**.

---

## Connection Pool

A connection pool works similarly to a thread pool.

When the application starts, a fixed number of database connections are created and stored in the pool.

Whenever a request requires a database connection:

1. A connection is borrowed from the pool.
2. The connection is used to execute database operations.
3. The connection is returned to the pool instead of being physically closed.

If all connections are currently in use, incoming requests wait until a connection becomes available. If a connection is not available within the configured timeout period, the request fails with a timeout exception.

Spring Boot uses **HikariCP** as its default connection pool implementation.

Common HikariCP settings include:

* Maximum pool size
* Minimum idle connections
* Connection timeout
* Idle timeout
* Maximum connection lifetime

These properties can be configured through application configuration files.

---

## Implementing Spring JDBC

Spring provides multiple dependencies for working with JDBC.

### Dependencies

#### 1. Spring JDBC (`spring-jdbc`)

* Provides core JDBC abstractions such as `JdbcTemplate`, `NamedParameterJdbcTemplate`, transaction support, and exception translation.
* Requires manual configuration of the `DataSource`, JDBC driver, and connection pool.
* Suitable when full control over configuration is required and Spring Boot auto-configuration is not being used.

#### 2. Spring Boot Starter JDBC (`spring-boot-starter-jdbc`)

* Provides `spring-jdbc`, transaction support, HikariCP, and Spring Boot auto-configuration.
* Requires only database connection properties such as URL, username, password, and JDBC driver.
* Suitable for Spring Boot applications that use `JdbcTemplate` directly while minimizing configuration.

For the remainder of this discussion, we will focus on **Spring Boot Starter JDBC**.

---

## Configuration

Spring Boot exposes `DataSource` properties under the `spring.datasource` namespace.

Connection pool properties are configured under `spring.datasource.hikari`.

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/localdb
    username: root
    password: password

    hikari:
      maximum-pool-size: 10
      connection-timeout: 30000
```

---

## Auto-Configuration Performed by Spring Boot

When Spring Boot starts, it performs the following steps automatically:

1. Detects Spring JDBC on the classpath.
2. Detects the JDBC driver.
3. Reads `spring.datasource` properties.
4. Detects an available connection pool implementation.
5. Prefers HikariCP when available.
6. Creates and configures a `DataSource`.
7. Registers the `DataSource` as a Spring bean.
8. Creates and registers a `JdbcTemplate` bean.

---

## JdbcTemplate

`JdbcTemplate` provides methods that simplify database operations.

### 1. update()

Used for INSERT, UPDATE, and DELETE operations.

```java
int rowsAffected = jdbcTemplate.update(sql, params);
```

Returns the number of rows affected.

---

### 2. query()

Used for SELECT operations that return multiple rows.

Since Spring does not know how to convert a database row into an object, a `RowMapper` must be provided.

`RowMapper<T>` is a functional interface that defines:

```java
T mapRow(ResultSet rs, int rowNum)
```

Example:

```java
List<Student> students =
        jdbcTemplate.query(
                sql,
                new StudentRowMapper(),
                params
        );
```

---

### 3. queryForObject()

Used for SELECT operations that are expected to return exactly one row.

```java
Student student =
        jdbcTemplate.queryForObject(
                sql,
                new StudentRowMapper(),
                params
        );
```

Possible outcomes:

* Returns the mapped object if exactly one row is found.
* Throws `EmptyResultDataAccessException` if no row is found.
* Throws `IncorrectResultSizeDataAccessException` if more than one row is returned.

---

## Built-in RowMapper

Spring provides `BeanPropertyRowMapper` to automatically map database columns to Java object properties.

```java
RowMapper<Student> rowMapper =
        new BeanPropertyRowMapper<>(Student.class);
```

It supports automatic conversion between:

```text
student_name  →  studentName
first_name    →  firstName
```

This eliminates the need to write custom `RowMapper` implementations for simple mappings.

---

## Exception Translation

JDBC drivers throw `SQLException`, which is a checked exception.

The problem with `SQLException` is that it is very generic. Determining the actual cause often requires examining database-specific error codes and SQL state values.

Spring JDBC catches these exceptions and translates them into meaningful, database-independent unchecked exceptions.

Common examples include:

1. `DuplicateKeyException`
2. `DataIntegrityViolationException`
3. `BadSqlGrammarException`
4. `EmptyResultDataAccessException`
5. `IncorrectResultSizeDataAccessException`
6. `CannotGetJdbcConnectionException`

This allows applications to handle database errors in a cleaner and more consistent way.

> Since these exceptions are unchecked exceptions, they can be handled centrally using a global exception handler.
