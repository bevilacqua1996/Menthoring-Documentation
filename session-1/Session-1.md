# Session 1

# **Java Mentoring - Session 1**
## **1. Basic Java Concepts and Version Evolution**
*   **JVM (Java Virtual Machine)**: Runs Java bytecode on any operating system.
*   **JDK (Java Development Kit)**: Includes the compiler, tools, and the JVM.
![](https://t90151464882.p.clickup-attachments.com/t90151464882/fbbd811c-7347-4071-8f9e-df82ce81e783/image.png)
*   **Basic syntax:**

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

*   **Collections:**
    *   **List**

```java
List<String> list = new ArrayList<>();
list.add("Apple");
list.add("Banana");
```

*       *   **Set**

```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
```

*       *   **Map**

```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 10);
map.put("Banana", 20);
```

*   **Java evolution:**
    *   **Java 8:** Lambdas, Streams API, Optional
    *   **Java 11:** `var`, new String APIs
        *   **`var`** **(Java 11+):**

```java
var name = "John"; // Type inferred automatically
```

*       *   **Java 17:** Records, Pattern Matching
        *   **Record (Java 17+):**

```java
public record Person(String name, int age) {}

var person = new Person("Mary", 30);
System.out.println(person.name());
```

*       *   **Java 21:** Sequenced Collections, Virtual Threads (preview)
    *   **Java 24:** Preview features
* * *
## **2. Lambdas and Functional Interfaces**
*   **Functional Interface**

```java
@FunctionalInterface
public interface Greeting {
    void say(String name);
}
```

*   **Lambda**

```java
Greeting greeting = name -> System.out.println("Hello " + name);
greeting.say("Peter");
```

*   **Streams API**
#### Pros and Cons: Streams vs Traditional For Loop
*       *       *   **Pros of using Streams:**
            *   More concise and readable code.
            *   Makes chained operations easier, such as filters, mappings, and groupings.
            *   Can take advantage of parallelism easily (`parallelStream()`).
            *   Less prone to errors from manual list manipulation.
*       *       *   **Cons of using Streams:**
            *   Can be less performant in simple cases due to stream creation overhead.
            *   Harder to debug step by step.
            *   Not always intuitive for people who do not know the API well.
*       *       *   **Pros of using a traditional for loop:**
            *   Full control over flow and variables.
            *   Easier to debug.
            *   Better performance in simple cases.
*       *       *   **Cons of using a traditional for loop:**
            *   More verbose code.
            *   More prone to errors, such as forgetting an increment or manipulating indexes.
            *   Harder to compose complex operations such as filters and groupings.
*       *       *   **Summary:**
            *   Use **streams** for more complex collection operations and when you want cleaner code.
            *   Use **for** when you need full control or are dealing with very simple cases.

```java
List<String> names = List.of("Ana", "Bruno", "Carlos");
names.stream()
     .filter(n -> n.startsWith("A"))
     .forEach(System.out::println);
```

* * *
## **3. Spring Boot - Introduction**
*   **Creating a project:**
    *   Go to [start.spring.io](http://start.spring.io)
    *   Configure the dependencies and download the project
*   **Folder structure:**
    *   `src/main/java` -> source code
    *   `src/main/resources` -> configuration files
*   **Example with** **`@RestController`** **and** **`@GetMapping`**

```java
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello, Spring Boot!";
    }
}
```

* * *
## **4. Practical Exercises**
1. Create a **record** to represent a product.
2. Create and use a **functional interface** to apply a discount.
3. Create a **List**, a **Set**, and a **Map**, and manipulate their data.
4. Create a REST endpoint in Spring Boot that returns a JSON object.
