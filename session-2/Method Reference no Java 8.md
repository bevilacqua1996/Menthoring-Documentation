# Method Reference in Java 8

**Method references** are a **shorter and more readable** way to write _lambdas_ when all they do is call an existing method.
General syntax:

```java
ClassOrObject::method
```

* * *
## Method Reference Types
### 1. Reference to a **static method**
When the method is `static`.

```java
Function<String, Integer> lambda = s -> Integer.parseInt(s);
Function<String, Integer> reference = Integer::parseInt;

System.out.println(reference.apply("456")); // 456
```

* * *
### 2. Reference to an **instance method of a specific object**
When an object already exists and you want to use one of its methods.

```java
String text = "java";

Supplier<Integer> lambda = () -> text.length();
Supplier<Integer> reference = text::length;

System.out.println(reference.get()); // 4
```

* * *
### 3. Reference to an **instance method of an arbitrary object**
The object comes implicitly from the parameter.

```java
List<String> names = Arrays.asList("bruno", "ana", "carlos");

names.forEach(name -> System.out.println(name));
names.forEach(System.out::println);
```

* * *
### 4. Reference to a **constructor**
Creating instances via a constructor reference.

```java
Supplier<Pessoa> lambda = () -> new Pessoa();
Supplier<Pessoa> reference = Pessoa::new;

reference.get(); // "New person created!"
```

* * *
## Summary
*   `Class::staticMethod` -> static method
*   `obj::instanceMethod` -> instance method of a specific object
*   `Class::instanceMethod` -> instance method of an arbitrary object
*   `Class::new` -> constructor
