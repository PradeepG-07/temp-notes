## Introduction
Spring MVC is Spring's web framework for building web applications and RESTful APIs.

Spring MVC builds on top of the Servlet API and provides higher-level abstractions that reduce boilerplate code and simplify web application development.

Spring MVC introduces annotations such as `@Controller`, `@RestController`, `@RequestMapping`, `@GetMapping`, and `@PostMapping` to make web development more declarative and easier compared to working directly with servlets.
## What is better in Spring MVC compared to Servlets?
1. Better routing and request handling
	- Spring MVC gives flexibility to map each method of a controller to a specific route along with specific HTTP method.
	- Request data like path variable, query parameters can be accessed easily.
2. Automatic JSON Serialization and Deserialization 
	 - JSON Parsing, mapping from JSON to Java objects and vice versa is handled automatically with help of libraries such as `Jackson`.
3. Global Exception handling is introduced with annotations like `ControllerAdvice` and `ExceptionHandler`.
4. Repetitive infra logic in controllers which is not related to business logic is removed and handled in `DispatcherServlet`. 
