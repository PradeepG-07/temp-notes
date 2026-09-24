## Hibernate with SpringBoot
After introducing JPA only the specification is created, but the boilerplate code still exists. SpringBoot removes this boilerplate code with help of AutoConfiguration.
- SpringBoot automatically creates an `EntityManagerFactory` instance and makes it available in IoC Container.
- Earlier to create an `EntityManager` we were using `EntityMangerFactory` in each method. But in SpringBoot we can use the annotation called `@PersistenceContext`.

## @PersistenceContext
This annotation is used on `EntityManager` attribute, this annotation will tell JPA to inject a new `EntityManager` object for every method call.

From here, the same flow occurs just like if `Session` was created, a new `PersistenceContext` will be attached to the `EntityManager` to manage the entities.

## @Transactional
In hibernate, we used to create a transaction and either commit or rollback manually. But spring boot provides `@Transactional` to handle the transaction.

Spring uses AOP to handle the transaction begin, commit and rollback. It uses `@Around` advice and wraps the original method call inside the proxy method.

Transactional handles the transactions within one application modifying a single row by multiple requests, for microservices it spans across db and should be handled in different ways.