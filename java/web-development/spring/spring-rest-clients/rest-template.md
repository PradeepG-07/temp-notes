# RestTemplate

`RestTemplate` is a synchronous HTTP client provided by Spring for consuming REST APIs. It follows the Template Method pattern and provides several overloaded methods for performing HTTP operations.

> **Note:** `RestTemplate` is in maintenance mode and is deprecated in favor of Spring's `RestClient` for new applications.

## Common Method Categories

### 1. GET Operations

#### `getForObject`

* Executes a GET request.
* Returns only the response body.
* Automatically converts the response to the specified type.

```java
getForObject(url, ResponseType.class)
```

#### `getForEntity`

* Executes a GET request.
* Returns a `ResponseEntity`.
* Useful when response status code or headers are required.

```java
getForEntity(url, ResponseType.class)
```

---

### 2. POST Operations

#### `postForObject`

* Executes a POST request.
* Returns only the response body.
* Request body and headers can be supplied using `HttpEntity`.

```java
postForObject(url, request, ResponseType.class)
```

#### `postForEntity`

* Executes a POST request.
* Returns a `ResponseEntity`.

```java
postForEntity(url, request, ResponseType.class)
```

#### `postForLocation`

* Executes a POST request.
* Returns the URI of the newly created resource from the `Location` header.

```java
postForLocation(url, request)
```

---

### 3. PUT Operations

#### `put`

* Executes a PUT request.
* Returns `void`.
* Typically used for complete resource updates.

```java
put(url, request)
```

> Unlike GET and POST, `RestTemplate` does not provide `putForObject` or `putForEntity`.

---

### 4. PATCH Operations

#### `patchForObject`

* Executes a PATCH request.
* Returns the response body.
* Typically used for partial resource updates.

```java
patchForObject(url, request, ResponseType.class)
```

> There is **no** `patchForEntity` method in `RestTemplate`.

If the response status or headers are required, use:

```java
exchange(url, HttpMethod.PATCH, entity, ResponseType.class)
```

#### Important Limitation

`patchForObject` does not work when `RestTemplate` is created using the default `SimpleClientHttpRequestFactory`.

Reason:

* `SimpleClientHttpRequestFactory` uses Java's legacy `HttpURLConnection`.
* `HttpURLConnection` does not support the HTTP PATCH method.

Solution:

* Configure `RestTemplate` with another request factory such as `HttpComponentsClientHttpRequestFactory`.

Reference:
https://docs.spring.io/spring-framework/reference/integration/rest-clients.html#rest-request-factories

---

### 5. DELETE Operations

#### `delete`

* Executes a DELETE request.
* Returns `void`.

```java
delete(url)
```

```java
delete(url, uriVariables)
```

> There is no `deleteForObject` or `deleteForEntity` method.

If the response body, status code, or headers are required, use `exchange()`.

---

### 6. Generic Operations

#### `exchange`

* Most flexible method in `RestTemplate`.
* Supports all HTTP methods.
* Allows sending headers, request body, path variables, and query parameters.
* Returns a `ResponseEntity`.

```java
exchange(
    url,
    HttpMethod.GET,
    entity,
    ResponseType.class
)
```

Commonly used for:

* PATCH requests
* Custom headers
* Authorization tokens
* Accessing response status and headers

---

### 7. execute

#### `execute`

* Lowest-level API provided by `RestTemplate`.
* Gives direct access to request and response processing.
* Used for advanced customizations and streaming scenarios.

```java
execute(url, HttpMethod.GET, requestCallback, responseExtractor)
```

---

## HttpEntity vs ResponseEntity

### HttpEntity

Represents an HTTP request.

Contains:

* Headers
* Body

```java
HttpEntity<RequestType> entity =
    new HttpEntity<>(requestBody, headers);
```

### ResponseEntity

Represents an HTTP response.

Contains:

* Status code
* Headers
* Body

```java
ResponseEntity<ResponseType> response
```

---

## Summary

| HTTP Method     | Method(s)       | Return Type       |
| --------------- | --------------- | ----------------- |
| GET             | getForObject    | Response Body     |
| GET             | getForEntity    | ResponseEntity    |
| POST            | postForObject   | Response Body     |
| POST            | postForEntity   | ResponseEntity    |
| POST            | postForLocation | URI               |
| PUT             | put             | void              |
| PATCH           | patchForObject  | Response Body     |
| DELETE          | delete          | void              |
| Any HTTP Method | exchange        | ResponseEntity    |
| Any HTTP Method | execute         | Custom Processing |

## Reference
[Spring Doc](https://docs.spring.io/spring-framework/reference/integration/rest-clients.html)