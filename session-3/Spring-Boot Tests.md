# Testing in Spring Boot

### Overview
Testing is essential to ensure your code works and keeps working after changes. We will look at test types, how to write them, best practices, and useful tools for everyday Java + Spring API work.
* * *
## 1. Unit Tests with JUnit and Mockito
### Concept
Unit tests validate the behavior of **isolated units** in the system, usually methods from service classes or utility classes.
### Tools
*   **JUnit 5**: Standard testing framework.
*   **Mockito**: Mocks external dependencies such as repositories and external services.
### Example

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {

    @Mock
    private UserRepository userRepository;

    @InjectMocks
    private UserService userService;

    @Test
    void shouldReturnUserById() {
        User mockUser = new User(1L, "John");
        when(userRepository.findById(1L)).thenReturn(Optional.of(mockUser));

        User user = userService.getUserById(1L);

        assertEquals("John", user.getName());
    }
}


```

* * *
## 2. Integration Tests
### `@SpringBootTest`
Starts the full application context. Ideal for testing integration between **real components** such as the database, controllers, and services.

```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerIT {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void shouldReturnUser() throws Exception {
        mockMvc.perform(get("/users/1"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$.name").value("John"));
    }
}


```

* * *
### `@WebMvcTest`
Starts only the web layer (controllers, filters). Ideal for testing **isolated controllers**, mocking the services behind them.

```java
@WebMvcTest(UserController.class)
class UserControllerWebTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    private UserService userService;

    @Test
    void shouldReturnUser() throws Exception {
        User user = new User(1L, "John");
        when(userService.getUserById(1L)).thenReturn(user);

        mockMvc.perform(get("/users/1"))
            .andExpect(status().isOk())
            .andExpect(header().exists("Content-Type"))
            .andExpect(jsonPath("$.name").value("John"));
    }
}


```

* * *
## 3. Testing HTTP status, headers, and body
### Practical example with `MockMvc`

```java
mockMvc.perform(post("/users")
        .content("{\\"name\\": \\"John\\"}")
        .contentType(MediaType.APPLICATION_JSON))
    .andExpect(status().isCreated())
    .andExpect(header().string("Location", containsString("/users/")))
    .andExpect(jsonPath("$.name").value("John"));


```

* * *
## Complementary Tools
### Postman
*   Ideal for manually testing REST endpoints.
*   Generates reusable **collections**.
*   Can be exported and used with CI/CD.
### RestAssured
*   Java library for testing REST APIs with fluent syntax.

```java
given()
    .contentType("application/json")
    .body("{\\"name\\": \\"John\\"}")
.when()
    .post("/users")
.then()
    .statusCode(201)
    .body("name", equalTo("John"));


```

### Testcontainers (advanced)
*   Spins up Docker containers (for example, databases) **during tests**.
*   Very useful for real integration tests with PostgreSQL, Mongo, Kafka, etc.

```java
@Testcontainers
class UserRepositoryIT {

    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:13");

    @DynamicPropertySource
    static void overrideProps(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }

    @Autowired
    private UserRepository userRepository;

    @Test
    void shouldPersistUser() {
        User user = new User(null, "Mary");
        User saved = userRepository.save(user);
        assertNotNull(saved.getId());
    }
}


```

* * *
## Final Tips
*   Start with unit tests, then move to integration tests.
*   Cover what is **critical** in the system, such as business rules.
*   Method names should indicate the expected behavior.
*   Slow tests? Use different profiles (`@ActiveProfiles("test")`).
*   Test failed? Fix **the code**, not the test.
