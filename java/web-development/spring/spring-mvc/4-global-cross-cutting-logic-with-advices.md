## Annotations
- Annotations used over here are `@ControllerAdvice` and `@RestControllerAdvice`.

- Any class which is declared with any of the above annotations will act as a helper class of all the controllers.

- ***Common logic of the controllers can be added in these helper classes***.
    - **Examples**: Exception Handling, Data Binding, Model Attributes for views.

## @ControllerAdvice
1. There can be multiple classes with this annotation to handle the exceptions.
2. If the same exception is being handled in two or more advices, then we can use scoping or ordering for better resolutions.
3. **Scoping**: `@ControllerAdvice(basePackages = "package1,package2,..etc""`.
4. **Ordering**: `@Order(orderValue)` can be used to execute the handler one after the other in a sequence.

   > Same holds valid for the `@RestControllerAdvice` as well. Differences btw these are similar to that of `@RestController` and `@Controller`
