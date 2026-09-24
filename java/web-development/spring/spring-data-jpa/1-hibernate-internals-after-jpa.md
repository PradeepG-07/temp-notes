# Changes After JPA Was Introduced

Before JPA, applications interacted directly with Hibernate-specific APIs such as `SessionFactory`, `Session`, and `Transaction`.

JPA introduced a standard persistence API, allowing applications to work with any JPA-compliant provider (Hibernate, EclipseLink, OpenJPA, etc.) without being tightly coupled to a specific implementation.

Hibernate still performs the actual work internally, but developers usually interact with JPA abstractions instead of Hibernate APIs.

---

## Hibernate vs JPA

| Hibernate API       | JPA Equivalent            |
|---------------------|---------------------------|
| `SessionFactory`    | `EntityManagerFactory`    |
| `Session`           | `EntityManager`           |
| `Transaction`       | `EntityTransaction`       |
| `session.get()`     | `entityManager.find()`    |
| `session.save()`    | `entityManager.persist()` |
| `session.delete()`  | `entityManager.remove()`  |
| `session.update()`  | `entityManager.merge()`   |
| `session.flush()`   | `entityManager.flush()`   |

Internally, Hibernate still uses a `Session`, but JPA exposes it through the `EntityManager` interface.

Conceptually:

```text
Application
      |
      v
EntityManager (JPA)
      |
      v
Session (Hibernate)
      |
      v
Persistence Context
      |
      v
Database
```

---

# JPA Core Components

## 1. EntityManagerFactory

`EntityManagerFactory` is the JPA equivalent of Hibernate's `SessionFactory`.

Typically, one `EntityManagerFactory` is created per application.

```java
EntityManagerFactory emf =
        Persistence.createEntityManagerFactory("myPU");
```

## 2. EntityManager

`EntityManager` is the JPA equivalent of Hibernate's `Session`. It provides methods to interact with entities and persistence context.

```java
EntityManager em =
        emf.createEntityManager();
```

---

## Persistence Context in JPA

The Persistence Context concept did not change with JPA.
The difference is that JPA exposes standard APIs to interact with it.

These methods are available on `EntityManager` and directly affect the Persistence Context.

### 1. persist()
Similar to save(), but follows JPA semantics and does not return the generated identifier. Makes a transient entity persistent and schedules an INSERT operation.

### 2. find()
Similar to `get()` of hibernate, immediately loads an entity from the database and returns null if no record exists.

### 3. remove()
Similar to `delete()` of hibernate, marks an entity for deletion and schedules a DELETE operation. Entity state moves to Removed state but entity still remains in PersistenceContext.

### 4. detach()
Removes a specific entity directly from the PersistenceContext. Object exists in heap memory and no data is deleted from the database. Entity state is moved to Detached and tracking is stopped for this entity object.

### 5. clear()
Removes all entities from the PersistenceContext.

### 6. merge()
`merge()` takes an entity as a parameter, it copies the state into a new managed instance and returns that managed instance and does not make the original object managed.

### 7. flush()
Similar to that of `flush` of hibernate. Synchronizes the Persistence Context with the database.

### 8. refresh()
Takes an entity as an argument and reloads entity state from the database. When reloaded current in-memory changes are discarded.

### 9. contains()
Similar to that of `contains` of hibernate. Checks whether an entity is currently in Managed state.

## Plain JPA Code Flow

The equivalent of the earlier Hibernate example in pure JPA is:

```java
EntityManagerFactory emf =
        Persistence.createEntityManagerFactory("myPU");

EntityManager em =
        emf.createEntityManager();

EntityTransaction tx =
        em.getTransaction();

try {

    tx.begin(); // Step 1

    Employee emp =
            em.find(Employee.class, 1L); // Step 2

    emp.setName("John"); // Step 3

    tx.commit(); // Step 4

} catch (Exception e) {

    tx.rollback(); // Step 5

} finally {

    em.close(); // Step 6
}
```
### Step 1 – Begin Transaction
* A new database transaction is started.
* The Persistence Context becomes associated with the transaction.
* Hibernate begins tracking entities that are loaded or persisted during this unit of work.

### Step 2 – Load Entity
* Hibernate first checks the Persistence Context to see whether the entity is already available.
* If the entity is not present, a `SELECT` query is executed against the database.
* The retrieved entity is stored in the Persistence Context and marked as **Managed**.

### Step 3 – Modify Entity
* Changes are made only to the in-memory Managed entity.
* No SQL is executed immediately when entity properties are modified.
* Hibernate records these changes and relies on Dirty Checking to detect them later.

### Step 4 – Commit Transaction

### Flush Phase
* Hibernate compares the current entity state with the original snapshot stored in the Persistence Context.
* Dirty Checking identifies modified fields and generates the required SQL statements.
* Generated SQL statements are executed, synchronizing the Persistence Context with the database.

### Commit Phase
* After a successful flush, the database transaction is committed.
* All executed changes become permanent in the database.
* The transaction boundary ends.

### Step 5 – Rollback Transaction
* The current transaction is aborted.
* Any pending changes that have not been committed are discarded.
* The database remains unchanged by the operations performed within the transaction.

### Step 6 – Close EntityManager
* The EntityManager lifecycle ends and the associated Persistence Context is destroyed.
* All Managed entities become Detached because they are no longer associated with a Persistence Context.
* Hibernate stops tracking changes made to those entities.

These are the primary JPA operations used to directly interact with and control the Persistence Context.
