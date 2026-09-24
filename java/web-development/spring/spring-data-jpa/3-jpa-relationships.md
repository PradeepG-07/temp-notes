# Relationships

Relationships define how data in one table is associated with data in another table. The number of instances of one entity that can be associated with instances of another entity is called **cardinality**.

Based on cardinality, relationships are classified as 1:1 (One-to-One), 1:N (One-to-Many), N:1 (Many-to-One), and M:N (Many-to-Many).

In relational databases, relationships are represented using foreign keys and join tables.

* **One-to-One (1:1)**: A foreign key is typically stored in one of the tables, often with a unique constraint.
* **One-to-Many (1:N)**: The foreign key is stored in the "many" side table.
* **Many-to-One (N:1)**: This is the inverse view of a one-to-many relationship; the foreign key remains in the many-side table.
* **Many-to-Many (M:N)**: A separate join table stores the foreign keys of both entities.

# Relationships in JDBC

When using JDBC, related data can be fetched either through multiple queries or by using SQL joins.

However, relationship management is largely manual in JDBC. Developers must handle foreign keys, join tables, loading associated entities, cascading operations (insert, update, delete), and object-relational mapping themselves.

# JPA Relationships

JPA relationships allow Java entities to reference one another while Hibernate translates those object references into foreign keys and join tables in a relational database.

Consider the following example. A `Student` has a reference to a `Department`.

```java
@Entity
class Student{
    Long id;
    String name;
    Department department;
}
```

Considering only the Java language, `Student` stores a reference to a `Department` object. However, since `Student` is an entity, it represents a table in the database.

A Java object reference cannot be stored directly in a relational table. This is where JPA relationships come into play. They tell Hibernate how to map object references to the underlying database representation.

## Relationship Cardinality

Cardinality answers one question:

> How many entities on one side can be associated with how many entities on the other side?

JPA supports four basic cardinalities:

| Relationship  | Meaning                                        | Common Example          |
|---------------|------------------------------------------------|-------------------------|
| `@OneToOne`   | One entity is associated with one other entity | User and Profile        |
| `@OneToMany`  | One entity is associated with many entities    | Department and Students |
| `@ManyToOne`  | Many entities are associated with one entity   | Students and Department |
| `@ManyToMany` | Many entities on both sides can be associated  | Student and Courses     |

## One-to-Many and Many-to-One Relationships

1:M and M:1 relationships are simply two views of the same relationship.

Suppose the Engineering department contains three students.

From the department's perspective, `One Department -> Many Students`, which is a `one-to-many` relationship. From the student's perspective, `Many Students -> One Department`, which is a `many-to-one` relationship.

The database does not create two separate relationships. Both directions normally describe the same foreign key. The foreign key is stored in the many-side table.

```text
student.department_id
```

## UniDirectional and BiDirectional Relationships

These relationships describe only the Java entities and do not apply to SQL.

### 1. UniDirectional Relationship

Only one Java entity contains a reference to the other entity. Therefore, the application can navigate in only one direction.

```java
class Student{
    Long id;
    String name;
    Department department;
}

class Department{

}
```

Here, the application can navigate from `Student` to `Department`, but navigation from `Department` to `Student` is not possible.

### 2. BiDirectional Relationship

Both entities contain references to each other, allowing navigation in both directions.

```java
class Student{
    Long id;
    String name;
    Department department;
}

class Department{
    Long id;
    String name;
    List<Student> students;
}
```

Here, the application can navigate from `Student` to `Department` and from `Department` to `Student` as well.

### Bidirectional fields do not synchronize themselves

The following two Java statements are independent of each other:

```java
student.setDepartment(dept);
dept.getStudents().add(newStudent);
```

As we already know, changes are tracked through the Persistence Context. If we add a new student to an existing department, the corresponding department entity will not automatically have the new student populated in its student list.

Therefore, it is necessary for the developer to manually add the student to the department's student list as well.

## Owning Side and Inverse Side

When the same database relationship is mapped in two entities, JPA requires one authoritative side.

### 1. Owning Side

The owning side is the side JPA uses to determine the database value of the relationship. It simply means that this side controls the database representation of the relationship.

If we represent the `Student` and `Department` relationship in the database, it would look as follows:

```text
Student(id, name, department_id)
Department(id, name)
```

Consider the following example:
```java
class Student{
    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;
}
class Department{}
student.setDepartment(dept);
```

Here, the student can change its department. Since the `Student` table maintains the foreign key (`department_id`), it controls the database relationship. Therefore, it becomes the authoritative side for JPA.

### 2. Inverse Side

The inverse side provides the opposite navigation path but does not independently control the database relationship.

```java
class Department{
    @OneToMany(mappedBy = "department")
    List<Student> students = new ArrayList<>();
}
```

## `@ManyToOne`

Consider the same `Student`–`Department` example. There exists a many-to-one relationship from `Student` to `Department`. To represent this relationship, we use the `@ManyToOne` annotation.

### Mapping with `@ManyToOne`

`@ManyToOne` tells JPA that the field references another entity and many student entities may reference the same department.

`@JoinColumn` specifies the foreign key column used to store the relationship. By default, the join column references the primary key of the target entity.

```java
class Student{
    private Long id;
    private String name;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;
}

static main(){
    Department dept = new Department();
    dept.setId(5);

    Student st = new Student();
    st.setDepartment(dept);
}
```

The above code results in the `department_id` column storing the value `5` in the database.

### Nullable and Mandatory Relationships

By default, the relationship accepts `null` values. However, if the requirement states that a student cannot exist without a department, we need to configure two parameters.

```java
@ManyToOne(optional = false)
@JoinColumn(name = "department_id", nullable = false)
private Department department;
```

These two settings express the same rule at different layers:

| Setting            | Layer                     | Meaning                                   |
|--------------------|---------------------------|-------------------------------------------|
| `optional = false` | JPA object model          | The association is required               |
| `nullable = false` | Generated database schema | The foreign key column must be `NOT NULL` |

### Insert Behavior

If a student is inserted with an existing department, the department will not be stored again. Instead, the student's foreign key column stores the department's primary key value.

If the department does not exist, we must first persist the department and then persist the student. This can also be done automatically using cascading.

```java
@ManyToOne(cascade = CascadeType.PERSIST)
```

### Update Behavior

If a student entity in the MANAGED state is updated with a new department, Hibernate issues an `UPDATE` SQL statement during the flush phase.

## `@OneToMany`

Consider the `Department` and `Student` example. One department can contain many students. To represent this relationship from the department side, we use the `@OneToMany` annotation.

### Mapping with `@OneToMany`

`@OneToMany` tells JPA that one entity can be associated with multiple entities.

```java
class Department{
    private Long id;
    private String name;

    @OneToMany(mappedBy = "department")
    private List<Student> students = new ArrayList<>();
}
```

The `mappedBy` attribute indicates that the relationship is controlled by the `department` field in the `Student` entity.

### Database Representation

The foreign key is not stored in the department table. Instead, it is stored in the student table.

```text
Department(id, name)

Student(id, name, department_id)
```

The `@OneToMany` side only provides navigation and does not control the database relationship.

### Insert Behavior

Adding a student to the collection does not automatically update the database relationship.

```java
department.getStudents().add(student);
```

The owning side must also be updated.

```java
student.setDepartment(department);
```

In a bidirectional relationship, both sides should be kept synchronized by the application.

### Update Behavior

If a student's department reference is changed, Hibernate updates the foreign key column in the student table during the flush phase.


## `@OneToOne`
A one-to-one relationship means that each entity can be associated with at most one entity on the other side. Consider the example of a `User` and a `Profile`. Each user is associated with exactly one profile, and each profile belongs to exactly one user. To represent this relationship, we use the `@OneToOne` annotation.

### Database design
```text
User(id, name)
Profile(id, name, user_id)
```
A foreign key alone does not guarantee one-to-one. Without a uniqueness constraint, the database can allow duplicates. That means `user_id` field can have same user id in multiple rows which means a user can have multiple profiles.

### Mapping with `@OneToOne`
```java
class Profile{
    @OneToOne
    @JoinColumn(name = "user_id", unique = true)
    private User user;
}
```
### Owning and Inverse Sides
`Profile` is the owning side because it contains the join column.
```java
@OneToOne
@JoinColumn(name = "user_id")
private User user;
```
The inverse side in `User` is:
```java
class User{
    @OneToOne(mappedBy = "user")
    private Profile profile;
}
```
Here `user` refers to `Profile.user`.

If profile should not exist without user then foreign key should be declared as not null. Hence, we use `optional=false` and `nullable=false`. 

### Insert Behavior

If a user is inserted with an existing profile, the profile is not stored again. Instead, the foreign key column stores the profile identifier. If the profile does not exist, it must be persisted before persisting the user. 

### Update Behavior

If a managed user entity is updated with a different profile, Hibernate issues an `UPDATE` SQL statement during the flush phase.

## `@ManyToMany`

Consider the example of `Student` and `Course`. A student can enroll in multiple courses, and a course can contain multiple students. Neither table can represent the relationship using one foreign key column. A third table is required.

```text
students(id, name)
courses(id, name)
student_courses(student_id, course_id)
```
The third table is also called as Join table or Association table or Junction table. The pair (student_id, course_id) should be unique to prevent student enrolling into same course multiple times.

To represent this relationship, we use the `@ManyToMany` annotation.

### Mapping with `@ManyToMany`

`@ManyToMany` tells JPA that multiple entities on one side can be associated with multiple entities on the other side.

`@JoinTable` describes the middle table.

| Attribute          | Meaning                                   |
|--------------------|-------------------------------------------|
| name               | Name of the join table                    |
| joinColumns        | Foreign key pointing to the owning entity |
| inverseJoinColumns | Foreign key pointing to the other entity  |

```java
class Student{

    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id"),
        uniqueConstraints = @UniqueConstraint(columnNames = {"student_id", "course_id"})
    )
    private Set<Course> courses;
}
```

### Owning Side and Inverse Side

One side must be designated as the owning side, and it contains the `@JoinTable` annotation.

```java
class Student{

    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id"),
        uniqueConstraints = @UniqueConstraint(columnNames = {"student_id", "course_id"})
    )
    private Set<Course> courses = new HashSet<>();
}
```

`Student.courses` is the owning side. Changes to this collection controls rows of `student_course`. `student.getCourses().add(newCourse)` will make hibernate issue a INSERT SQL command when student is persisted.

The `Course` side provides the inverse navigation path.
```java
class Course{

    @ManyToMany(mappedBy = "courses")
    private Set<Student> students;
}
```

### Managing Both Sides

When a student is associated with existing courses, Hibernate inserts rows into the join table.

`student.getCourses().add(course);` This creates an entry in the join table containing the student identifier and course identifier.

But doing only this will create inconsistency in the persistence context hence the inverse side also has to be updated.

> Note: Whatever updates or inserts are done should be done on both sides.
