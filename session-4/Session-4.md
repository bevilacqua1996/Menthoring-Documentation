# Session 4

### What Are Design Patterns and Why Do They Matter
💡 **Design Patterns** are reusable solutions for common problems in architecture and code organization.
They help write cleaner, more modular, and more testable code.
**Benefits:**
*   Avoid reinventing the wheel
*   Make maintenance and testing easier
*   Give the team a common vocabulary
## Most Common Patterns in Spring Boot APIs
### Builder Pattern - _Used in DTOs and Responses_
**When to use:**
When the object has many attributes and we want to build it in a fluent and readable way.

```java
UserResponse user = UserResponse.builder()
    .name("John")
    .email("john@example.com")
    .active(true)
    .build();


```

**Advantages:**
*   Avoids constructors with many parameters
*   More readable and cleaner code
### Factory Pattern - _Creation of complex objects_
**When to use:**
When we want to encapsulate the creation of objects that change based on a parameter.

```java
public class NotificationFactory {
    public static Notifier create(String type) {
        return switch (type) {
            case "EMAIL" -> new EmailNotifier();
            case "SMS" -> new SmsNotifier();
            default -> throw new IllegalArgumentException("Invalid type");
        };
    }
}


```

**Advantages:**
*   Centralizes creation logic
*   Reduces coupling between components
### Strategy Pattern - _Variable behaviors_
**When to use:**
When we have several ways to execute the same behavior, such as calculations or business rules.

```java
public interface TaxCalculator {
    BigDecimal calculate(BigDecimal amount);
}

@Service
public class BrazilTax implements TaxCalculator { ... }

@Service
public class USATax implements TaxCalculator { ... }

@Service
public class TaxService {
    private final Map<String, TaxCalculator> strategies;

    public TaxService(List<TaxCalculator> calculators) {
        this.strategies = calculators.stream()
            .collect(Collectors.toMap(
                c -> c.getClass().getSimpleName().toLowerCase(),
                Function.identity()));
    }

    public BigDecimal calculate(String country, BigDecimal amount) {
        return strategies.get(country.toLowerCase()).calculate(amount);
    }
}


```

**Advantages:**
*   Replaces structures with many `if/else`
*   Easy to extend with new rules (Open/Closed Principle)
### Template Method - _Flows with variations_
**When to use:**
When there is a fixed flow, but specific parts change.

```java
public abstract class ReportGenerator {
    public final void generate() {
        loadData();
        process();
        export();
    }

    protected abstract void loadData();
    protected abstract void process();
    protected abstract void export();
}


```

**Advantages:**
*   Enforces a standard structure
*   Avoids duplication and makes maintenance easier
## When **NOT** to Use a Pattern
❗ **Overengineering is real.**
Design Patterns are meant to solve real problems. Using a pattern when it is not needed can make the code harder than it should be.
**Avoid it if:**
*   The simple solution already works
*   The pattern makes the code more confusing
*   The team is not familiar with it
👉 _Less is more. Clarity > complexity._
## Final Tip
> Key question before applying any pattern:  
> **"Does this pattern solve a real problem here?"**
If yes, go for it.
If not, stick with the simple solution. The best pattern is the one that makes sense for the context.

### What Is the Mapper Pattern (Data Mapper)
> The Mapper Pattern isolates the conversion logic between domain objects (or entities) and data objects (such as DTOs, views, forms, etc.).
* * *
### Typical Use Example (Java + Spring Boot)

```java
public class UserMapper {

    public static User toEntity(UserDTO dto) {
        return new User(dto.getName(), dto.getEmail());
    }

    public static UserDTO toDTO(User entity) {
        return new UserDTO(entity.getName(), entity.getEmail());
    }
}


```

* * *
### When Is This a **design pattern**?
If you:
*   **Centralize** conversion in a dedicated layer
*   **Isolate** mapping logic
*   **Avoid coupling** between entities and the external representation (DTOs)
Then yes, you are applying the **Data Mapper Pattern** - especially useful in layered architecture projects.
* * *
### Comparing With Other Patterns:

| Pattern | Main purpose |
| ---| ---|
| **Mapper** | Convert between data representations |
| **Adapter** | Adapt different interfaces |
| **Decorator** | Add functionality to objects |
| **Factory** | Create instances flexibly |

* * *
### Advantages of the Mapper Pattern
*   Isolates conversion responsibility
*   Avoids transformation logic in the controller or service
*   Makes testing and maintenance easier
* * *
### When **not** to overdo it
*   Avoid very generic mappers that are hard to test
*   If DTOs and entities are identical, a mapper may just add complexity
* * *
💡 Tools such as **MapStruct** or **ModelMapper** automate this, but the concept behind them is still the same pattern.
