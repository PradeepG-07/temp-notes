## Annotations

1. Java annotations are used to provide metadata for Java code
2. Annotations are added in Java 5.
3. Typically used for the following purposes
    1. Compile time instructions
    2. Build time instructions
    3. Runtime instructions
4. Usually annotations are not present in class files after the source code compilation and this behavior can be controlled explained in later steps(`@Retention`).
5. **Usage:** Annotation will start with `@` sign and can also have members called **elements.**
    1. @Deprecated, @Deprecated( since = “9”), @Deprecated(“9”).
    2. If annotation has only one element as a convention it will be named as **value().**
6. **Placements:** Annotation can be placed on class, method, instance variables, local variables, static variables, function parameters. This placement can be controlled as well. Explanation below(`@Target`).
7. Some annotations used to annotate annotations are `@Retention` `@Target` `@Documented` `@Inherited`
8. **Built In Annotations**
    1. **`@Override` :** Placed on top of the overridden methods, not mandatory but will help us to identify when the parent class / interface have changed the abstract method signature.
    2. **`@Deprecated` :** Placed on top of constructor, field, local variable, method, package, module, parameter, type(class, interface, enum). Tells the developer that the usage is deprecated use the updated one.
    3. `@**SuppressWarnings` :** Tells compiler to ignore the warnings mentioned in the annotation.
        1. Example: @SuppressWarnings("unchecked"), @SuppressWarnings({"all", "unchecked", "deprecation"})
    4. `@**Contended` :** It is used to avoid the false sharing a concurrency degradation problem.
9. **Custom Annotations**
    1. Simple Custom Annotation: `public @interface CustomAnnotation{}`
    2. Annotation with restrictions on placement, availability

        ```java
        // Controls till where annotation should be available.
        @Retention(RetentionPolicy.RUNTIME)
        // Controls the placement of the annotation
        @Target(ElementType.METHOD) // single type
        @Target({ElementType.FIELD, ElementType.TYPE} // Multiple types
        public @interface CustomAnnotation{
        }
        ```

    3. Annotation with elements

        ```java
        public @interface CustomAnnotation1{
        	String value();
        }
        
        public @interface CustomAnnotation2{
        	String name() default "";
        	String[] tags;
        }
        
        public @interface CustomAnnotation3{
        	int since;
        	String[] tags;
        }
        
        // Usage
        @CustomAnnotation1(value = "something") or @CustomAnnotation("something")
        @CustomAnnotation2(tags = {"java", "javascript"})
        @CustomAnnotation3(since = 10, tags = {"java", "javascript"})
        ```


More on `Retention` , `Target` are here. https://jenkov.com/tutorials/java/annotations.html#accessing-java-annotations-via-java-reflection