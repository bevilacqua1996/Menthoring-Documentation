# Relationships in Spring Data JPA

In this short lesson we will understand how to map relationships between entities in **Spring Boot with JPA/Hibernate**.
* * *
## 1. One To Many / Many To One
👉 Classic example: **One Customer has many Orders**.
### [**Customer.java**](http://Customer.java)

```java
import jakarta.persistence.*;
import java.util.List;

@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    // One customer has many orders
    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL)
    private List<Order> orders;

    // getters and setters
}
```

### [**Order.java**](http://Order.java)

```java
import jakarta.persistence.*;

@Entity
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String description;

    // Many orders belong to one customer
    @ManyToOne@JoinColumn(name = "customer_id")
    private Customer customer;

    // getters and setters
}
```

📌 **Summary:**
*   `@OneToMany` -> the "one" side (Customer)
*   `@ManyToOne` -> the "many" side (Order)
*   `@JoinColumn` -> defines the **foreign key** in the `order` table.
* * *
## 2. Many To Many
👉 Example: **Students and Courses**.
One student can be enrolled in several courses, and one course can have several students.
### [**Student.java**](http://Student.java)

```java
import jakarta.persistence.*;
import java.util.Set;

@Entity
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToMany
    @JoinTable(
        name = "student_course",
        joinColumns = @JoinColumn(name = "student_id"),
        inverseJoinColumns = @JoinColumn(name = "course_id")
    )
    private Set<Course> courses;

    // getters and setters
}
```

### [**Course.java**](http://Course.java)

```java
import jakarta.persistence.*;
import java.util.Set;

@Entity
public class Course {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    @ManyToMany(mappedBy = "courses")
    private Set<Student> students;

    // getters and setters
}
```

📌 **Summary:**
*   `@ManyToMany` creates an **intermediate table** (`student_course`).
*   `@JoinTable` defines the join column names.
*   `mappedBy` indicates the mirrored side of the relationship.
* * *
## 3. Difference Between the Relationships

| Type | Real Example | How it works in JPA |
| ---| ---| --- |
| **OneToMany** | Customer -> Orders | One customer can have many orders. |
| **ManyToOne** | Order -> Customer | Many orders are associated with a single customer. |
| **ManyToMany** | Student <-> Course | One student can be in several courses, and one course can have several students. |

* * *
## 4. Important Notes
*   Always think about **which side owns the relationship** -> usually the side that has `@JoinColumn`.
*   Use `cascade = CascadeType.ALL` when you want to save child entities together with the parent.
*   `fetch = FetchType.LAZY` (the default for collections) avoids loading everything at once -> good for performance.
* * *
👉 With this, you can already model the main relationships between entities in **Spring Boot with JPA** 🚀.
