## Data Transfer Objects
Data Transfer Objects which are nothing but simple POJO classes that will handle the request and response data.

1. Consider a `Student` entity has `name`, `email`, `rollNumber`, `createdAt`, `updatedAt`, `isDeleted` attributes.
2. Usually we will map the `Student` entity directly with the `RequestBody` and send the same object as a response to the client.
3. But if we observe the attributes `createdAt`, `updatedAt` and `isDeleted` shouldn't be exposed to outside world.

Hence, we need some abstract mapper classes to handle the requests and responses of `Student`. Those are handled by Data Transfer Objects.

## Mapper Classes
Mapper classes are nothing but simple POJO classes which will handle the functionality of converting the DTO to Entity Object for an incoming request and converting an Entity Object to a DTO for sending a response to client.

## Data Validations
We can use `spring-boot-starter-validation` as a depedency to handle the validations.

### List of validation annotations
| Annotation | Used For Type | Description |
| ---------- | ------------- | ----------- |
| @NotNull | Any Object | Value cannot be `null` |
| @NotEmpty | String, Collection, Array | Value cannot be `null` or empty |
| @NotBlank | String | Value cannot be `null`, empty or only spaces |
| @Email | String | Value should be in valid email format |
| @Pattern(regexp) | String | Value should match the regex pattern |
| @Size(min, max) | String, Collection | Value length should be within the range |
| @Min(value) | Number | Cannot be < value |
| @Max(value) | Number | Cannot be > value |
| @Positive | Number | Cannot be <= zero |
| @Past | Date/time | Date should be in the past |
| @Future | Date/time | Date should be in the future |

- When validation is failed spring boot throws an exception.
- To add a custom message each annotation provides `message` attribute.
    ```java 
    @NotBlank(message = "Name cannot be null or empty or blank") 
    ```

> Validations will be triggered only when **@Valid** is applied before the variable. 

## TODO
- [ ] Know how to use DTO properly with statusCode, message, data fields.
- [ ] Know more about data validation like 
    - [ ] more annotations, 
    - [ ] exceptions thrown(`MethodArgumentNotValidException` or `BindException` or `ConstraintViolationException`)
    - [ ] Nested validations, collection validations, custom validations
    - [ ] Handling raised exceptions
    - [ ] DTO vs Entity validation, Validation Groups
    - [ ] Diff btw @Valid, @Validated