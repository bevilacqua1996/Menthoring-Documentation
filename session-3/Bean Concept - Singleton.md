# Bean Concept - Singleton

## Difference Between Singleton and `@Autowired` in Spring Boot
### **1. Singleton**
*   **Definition:**
*   A design pattern where there is only **one instance** of a class during the entire application lifecycle.
*   **In Spring:**
*   All _beans_ are **singleton by default**.
Example:

```java
@Service
public class MyService {
    // Only one instance of this class is created by Spring
    public String doSomething() {
        return "Running service";
    }
}
```

* * *
### **2. `@Autowired`**
*   **Definition:**
*   Annotation used for **dependency injection** in Spring.
*   Tells Spring to fetch the instance (usually singleton) and inject it into the class that needs it.
Example:

```java
@RestController
public class MyController {

    @Autowired
    private MyService myService;  // Spring injects the single instance here

    @GetMapping("/test")
    public String test() {
        return myService.doSomething();
    }
}
```

* * *
### **3. When a Bean Is NOT Singleton**
If we want a new instance to be created **every time it is injected**, we use another _scope_, such as `prototype`:

```java
@Service
@Scope("prototype")
public class MyPrototypeService {
    public String doSomething() {
        return "New instance created!";
    }
}
```

And in the controller:

```java
@RestController
public class MyPrototypeController {

    @Autowired
    private MyPrototypeService myPrototypeService;

    @GetMapping("/test-prototype")
    public String test() {
        return myPrototypeService.doSomething();
    }
}
```

📌 **Behavior:**
*   `Singleton`: One instance only -> reused throughout the app.
*   `Prototype`: A new instance created at each injection.
* * *
### **4. Summary**

| Concept | What it is | How Spring uses it by default |
| ---| ---| --- |
| **Singleton** | A single instance of the class | All beans are singletons |
| **@Autowired** | Automatically injects dependencies | Injects the instance managed by Spring |
