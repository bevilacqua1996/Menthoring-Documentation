# Using OpenAPI in Spring

## Goal
*   Understand what OpenAPI is and how it helps REST API development
*   Learn how to integrate and configure automatic documentation generation using `springdoc-openapi`
*   Navigate the interactive documentation (Swagger UI)
*   Improve communication and standardization within the team
* * *
## What is OpenAPI?
> OpenAPI is a standardized specification for describing RESTful APIs.  
> Formerly known as Swagger, it is used to generate interactive documentation and make APIs easier to consume by other teams or applications.
### Advantages:
*   Automatic and always up-to-date documentation
*   Interactive interface (Swagger UI)
*   Makes manual testing and client generation easier
*   Standardizes contracts between services
* * *
## Spring Boot Integration
### 1. Dependency (pom.xml)

```gherkin
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.5.0</version>
</dependency>
```

> This library adds Swagger UI automatically at `/swagger-ui.html` and generates the spec at `/v3/api-docs`.
* * *
### 2. Running the application
After starting the project, open:
*   📚 Documentation: [`http://localhost:8080/swagger-ui.html`](http://localhost:8080/swagger-ui.html)
*   🧾 Spec JSON: [`http://localhost:8080/v3/api-docs`](http://localhost:8080/v3/api-docs)
* * *
## Customizing the Documentation
### 1. Useful annotations

```less
@Operation(summary = "Find customer by ID", description = "Returns a customer's data")
@ApiResponse(responseCode = "200", description = "Customer found")
@ApiResponse(responseCode = "404", description = "Customer not found")


```

### 2. Controller example

```java
@RestController
@RequestMapping("/api/v1/customers")
public class CustomerController {

    @Operation(summary = "List all customers")
    @GetMapping
    public ResponseEntity<List<CustomerDTO>> listCustomers() {
        return ResponseEntity.ok(List.of(new CustomerDTO("John", "john@email.com")));
    }

    @Operation(summary = "Create new customer")
    @ApiResponse(responseCode = "201", description = "Customer created successfully")
    @PostMapping
    public ResponseEntity<CustomerDTO> createCustomer(@RequestBody @Valid CustomerDTO customer) {
        return ResponseEntity.status(HttpStatus.CREATED).body(customer);
    }
}


```

* * *
## Tips for Real Projects
*   Use `@Schema(description = "")` on DTOs to describe fields
*   Group endpoints with tags: `@Tag(name = "Customers", description = "Customer management")`
*   Document error codes and response examples
*   Combine with contract tests if possible (for example: [Rest Assured + OpenAPI validator])
* * *
## Benefits for the Team
*   Makes life easier for API consumers (frontend, mobile, third parties)
*   Removes the dependency on "document it later"
*   Reduces communication noise
*   Helps new developers understand the API visually
