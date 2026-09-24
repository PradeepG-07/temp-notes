# Abstract Classes

1. A *Java abstract class* is a class which cannot be instantiated and purpose of abstract classes is to function as base classes which can be extended by subclasses to create a full implementation.
2. Abstract classes or methods are declared with a `abstract` keyword.
3. Abstract classes can contain both abstract and non-abstract methods but if the class contains any one abstract method then that class must be marked as abstract.
4. Abstract methods in the abstract class are declared without any definition.
5. All the subclasses which is extending abstract super class must implement(override) the abstract methods of abstract super class or the subclass must be declared as abstract itself.
6. Helps in managing repetitive code.
- **Code**

    ```java
    public abstract class URLProcessorBase {
        public void process(URL url) throws IOException {
            URLConnection urlConnection = url.openConnection();
            InputStream input = urlConnection.getInputStream();
    
            try{
                processURLData(input);
            } finally {
                input.close();
            }
        }
        protected abstract void processURLData(InputStream input)
            throws IOException;
    }
    ```