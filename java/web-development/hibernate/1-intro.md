# Introduction
In Spring JDBC, we have eliminated the redundant code issues arisen by JDBC but still there are some manual tasks remaining.

## Manual Tasks From Spring JDBC
1. **Manual SQL:** To perform any persistence operation we have to write manual sql queries. It gives precise control but problem appears when there are hundreds of entities, many relationships and repeated CRUD operations. 
2. **Manual Parameter Binding:** Parameter values should be passed to `PreparedStatement` in the correct order. A wrong order may cause compilation issues.
3. **Manual Row to Object Mapping:** When reading data from DB the conversion mapper is required.

## Object Relational Impedance Mismatch
The difference between object models and relational models are collectively called as **Object Relational Impedance Mismatch**

## Introduction to ORM
Object Relational Mapping or ORM, is the process of defining how:
```text
Java Classes map to database tables
Java Objects map to database rows
Java Fields map to database columns
Java References map to foreign-key relationships
Java Collections map to related rows
```
### Responsibilities of ORM Framework
1. Mapping Metadata of classes and fields with tables and columns
2. SQL Generation
3. Parameter Binding
4. ResultSet Mapping
5. Relationship Management
6. Caching
7. Transaction Integration

## Hibernate ORM
Hibernate is an ORM framework which is built to eliminate the manual tasks to be performed. Hibernate implements the Jakarta Persistence Specification.

Here JPA(Java/Jakarta Persistence API) is a contract and the implementation of JPA is Hibernate. In Spring, JPA provides interfaces such as `Entity`, `Id`, `EntityManager`, `GeneratedValue` etc. and Hibernate provides objects at runtime.

## Setting Up Hibernate
### 1. Dependencies
Spring Data JPA - We use `spring-boot-starter-data-jpa` which internally brings hibernate ORM. Because spring uses hibernate as default ORM.
MySQL Driver for database.
### 2. Configuration
In `application.properties` we will configure the following to work with hibernate:

#### 1. DataSource Configuration
```text
spring.datasource.url=
spring.datasource.username=
spring.datasource.password=
```
#### 2. Schema Generation with `ddl-auto`
```text
spring.jpa.hibernate.ddl-auto=update
```
Common values are:
1. `none`: Hibernate performs no schema action
2. `validate`: Hibernate checks whether the existing schemas match the entity mappings. If they don't match hibernate throws an error. Also, no schemas are created or updated.
3. `create`: Hibernate always drops and recreates the mapped schemas when application starts.
4. `update`: Hibernate attempts to update the schema to match the mappings. 
5. `create-drop`: Hibernate creates schema at startup and drops it when the persistence factory closes.

In production, schemas changes are usually managed using tools such as **Flyway or Liquibase** rather than depending on `update`.

#### 3. SQL Logging
```text
spring.jpa.show-sql=true // enables sql logging
spring.jpa.properties.hibernate.format_sql=true // format sql queries
```
### 3. Annotations
#### 1. `@Entity`:
It marks a class as a persistent domain object that Hibernate/JPA should map to a database table and manage throughout its lifecycle.

#### 2. `@Id`:
Marks a field as the primary key of an entity, uniquely identifying each record in the database. When a class is annotated with `@Entity`, the class must have `@Id`.

#### 3. `@GeneratedValue`:
It is specifically for generating entity identifier (primary key) values, not ordinary columns.

| Strategy                  | One-line Definition                                                                        |
|---------------------------|--------------------------------------------------------------------------------------------|
| `GenerationType.AUTO`     | Hibernare decides which one to use.                                                        |
| `GenerationType.IDENTITY` | Uses the database's auto-increment/identity column feature to generate primary key values. |
| `GenerationType.SEQUENCE` | Uses a database sequence object to generate unique primary key values.                     |
| `GenerationType.TABLE`    | Uses a separate database table to store and generate primary key values.                   |

#### 4. `@Table`:
`@Table` maps an entity to a specific database table and configures table-level metadata.

| Property            | Simple Meaning         |
|---------------------|------------------------|
| `name`              | Table name             |
| `schema`            | Table folder/namespace |
| `catalog`           | Database name          |
| `uniqueConstraints` | Unique rules           |
| `indexes`           | Search optimization    |

```java
@Table(
    name = "employees",
    schema = "hr",
    catalog = "company_db",
    uniqueConstraints = {
        @UniqueConstraint(
            name = "uk_employee_email",
            columnNames = "email"
        )
    },
    indexes = {
        @Index(
            name = "idx_employee_name",
            columnList = "name"
        )
    }
)
public class Employee {
}
```
#### 5. `@Column`:
Specifies how an entity field is mapped to a database column and allows customization of column properties.

| Property           | Simple Meaning     |
|--------------------|--------------------|
| `name`             | Column name        |
| `nullable`         | Allow NULL?        |
| `unique`           | Must be unique?    |
| `length`           | String size limit  |
| `precision`        | Total digits       |
| `scale`            | Decimal digits     |
| `insertable`       | Include in INSERT? |
| `updatable`        | Include in UPDATE? |
| `columnDefinition` | Custom SQL type    |

```java
@Column(
    name = "employee_name",
    nullable = false,
    unique = true,
    length = 100,
    insertable = true,
    updatable = true,
    columnDefinition = "VARCHAR(100)"
)
private String name;

@Column(
    precision = 10,
    scale = 2
)
// For decimals it is recommended to use big decimal
private BigDecimal salary;
```
#### 6. `@Enumerated`:
Specifies how a Java enum should be stored in the database.

| Value              | Stored in DB                     |
|--------------------|----------------------------------|
| `EnumType.STRING`  | Enum name (`ACTIVE`, `INACTIVE`) |
| `EnumType.ORDINAL` | Enum position (`0`, `1`, `2`)    |

```java
public enum Status {
    ACTIVE,
    INACTIVE
}

@Enumerated(EnumType.STRING)
private Status status; // In Database it stores ACTIVE for Active and INACTIVE for Inactive as strings

@Enumerated(EnumType.ORDINAL)
private Status status; // In Database it stores 0 for Active and 1 for Inactive
```
#### 7. `@Lob`:
Maps a field to a Large Object (LOB) column for storing large amounts of text or binary data.
```java
@Lob
private String description; // Stored as large text type (TEXT, CLOB, etc.)
@Lob
private byte[] document; // Stored as a binary large object (BLOB)
```
#### 8. `@Transient`:
Marks a field so that JPA/Hibernate does not persist it to the database.
#### 9. `@Convert`:
Specifies a custom converter to transform a field's value between its Java representation and database representation.

**Entity.java**
```java
@Convert(converter = StatusConverter.class)
private Status status;
```
**StatusConverter.java**
```java
@Converter
public class StatusConverter
        implements AttributeConverter<Status, String> {

    public String convertToDatabaseColumn(Status status) {
        return status.getCode();
    }

    public Status convertToEntityAttribute(String value) {
        return Status.fromCode(value);
    }
}
```
| Property            | Simple Meaning         |
|---------------------|------------------------|
| `converter`         | Which converter to use |
| `disableConversion` | Turn conversion off    |

#### 10. `@Embeddable` and `@Embedded`
`@Embeddable`: Marks a class whose fields can be embedded into an entity and stored as part of the entity's table.
```java
@Embeddable // Reusable group of columns
public class Address {

    private String city;
    private String state;
}
```
`@Embedded`: Embeds an `@Embeddable` object into an entity so its fields are stored in the entity's table.
No separate table is created for the `@Embeddable` class.
```java
@Entity
public class Employee {

    @Id
    private Long id;

    @Embedded // Include embeddable fields here
    private Address address; 
}
```
**Database table**
```text
employee
---------------------
id
city
state
```
#### 11. `@AttributeOverrides` and `@AttributeOverride`
`@AttributeOverride`: Overrides the column mapping of a field inside an embedded (`@Embeddable`) object.
```java
@Embedded
private Address currentAddress;

@Embedded
@AttributeOverride(
    name = "city",
    column = @Column(name = "permanent_city")
)
private Address permanentAddress;
```
`@AttributeOverrides`: Groups multiple `@AttributeOverride` annotations together.`
```java
@Embedded
@AttributeOverrides({
    @AttributeOverride(
        name = "city",
        column = @Column(name = "permanent_city")
    ),
    @AttributeOverride(
        name = "state",
        column = @Column(name = "permanent_state")
    )
})
private Address permanentAddress;
```
#### 12. `@ElementCollection` and `@CollectionTable`

## Performing CRUD
1. To perform CRUD operations we will use `PersistenceManager` class which provides methods to do them.
2. `@PersistenceContext` annotation is used to inject a JPA `EntityManager` that is associated with the current persistence context. `@AutoWired` can also be used to inject this bean but `@PersistenceContext` is a standard way to do this.

### Some important Methods
### 1. persist()
Takes an entity object and saves it to the database.
### 2. remove()
Takes an entity object or a primary key and removes it from the database.
### 3. find()
Takes a primary key and removes it from the database.

> To Update we don't have any method, we will just find the entity object and make changes to it which automatically gets saved.
