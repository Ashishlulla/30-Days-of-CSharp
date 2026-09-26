# Day 26 — Reflection, Attributes & Records in C#

## Overview

Today I explored three important C# features:

* **Reflection** — Inspecting types and metadata at runtime
* **Attributes** — Adding metadata to program elements
* **Records** — Creating data-oriented types with value-based equality

These features are widely used in modern .NET applications, frameworks, serializers, testing libraries, dependency injection systems, and application infrastructure.

---

# 1. Reflection

## What is Reflection?

Reflection allows a C# application to inspect information about types at runtime.

Using Reflection, we can inspect:

* Classes
* Properties
* Methods
* Constructors
* Fields
* Interfaces
* Attributes

The main type used for Reflection is `System.Type`.

---

## Basic Reflection Example

```csharp
string name = "Ashish";

Type type = name.GetType();

Console.WriteLine(type.Name);
```

Output:

```text
String
```

`GetType()` returns information about the runtime type of an object.

---

## typeof()

We can also get type information using `typeof`.

```csharp
Type type = typeof(Employee);

Console.WriteLine(type.Name);
```

Example class:

```csharp
public class Employee
{
    public int Id { get; set; }

    public string Name { get; set; }

    public void Work()
    {
        Console.WriteLine("Employee is working");
    }
}
```

---

## Inspecting Properties

```csharp
Type type = typeof(Employee);

var properties = type.GetProperties();

foreach (var property in properties)
{
    Console.WriteLine(property.Name);
}
```

Output:

```text
Id
Name
```

---

## Inspecting Methods

```csharp
Type type = typeof(Employee);

var methods = type.GetMethods();

foreach (var method in methods)
{
    Console.WriteLine(method.Name);
}
```

Reflection can discover methods available on a type at runtime.

---

## Creating Objects Using Reflection

Reflection can also be used to create an instance dynamically.

```csharp
Type type = typeof(Employee);

object? employee = Activator.CreateInstance(type);
```

The exact approach depends on the constructor and required parameters.

---

## Where is Reflection Used?

Reflection is commonly used by:

* Dependency Injection containers
* Serialization libraries
* ORMs
* Testing frameworks
* Plugin systems
* Mapping libraries
* Application frameworks

For example, a framework may inspect a class to discover its properties or attributes without knowing the class structure at compile time.

---

## Reflection Performance

Reflection is powerful, but it can be slower and more complex than direct code.

Therefore:

* Use it when runtime inspection is actually required.
* Avoid unnecessary Reflection in performance-critical code.
* Cache reflection information when appropriate.

---

# 2. Attributes

## What are Attributes?

Attributes allow us to add metadata to code elements.

They can be applied to:

* Classes
* Methods
* Properties
* Parameters
* Fields
* Assemblies

Attributes are commonly written using square brackets.

```csharp
[Obsolete]
public void OldMethod()
{
}
```

---

## Built-in Attributes

C# and .NET provide many built-in attributes.

### Obsolete

```csharp
[Obsolete("Use NewMethod instead")]
public void OldMethod()
{
}
```

Calling the method produces a compiler warning.

---

## AttributeUsage

When creating a custom attribute, `AttributeUsage` can specify where the attribute is allowed.

```csharp
[AttributeUsage(AttributeTargets.Class)]
public class DeveloperAttribute : Attribute
{
}
```

This attribute can be applied to classes.

---

# 3. Custom Attributes

We can create our own attributes by inheriting from `Attribute`.

```csharp
public class AuthorAttribute : Attribute
{
    public string Name { get; }

    public AuthorAttribute(string name)
    {
        Name = name;
    }
}
```

Use the attribute:

```csharp
[Author("Ashish")]
public class Employee
{
}
```

The attribute stores metadata about the class.

---

## Reading Attributes Using Reflection

```csharp
Type type = typeof(Employee);

var attribute = type
    .GetCustomAttributes(typeof(AuthorAttribute), true)
    .FirstOrDefault();

if (attribute is AuthorAttribute author)
{
    Console.WriteLine(author.Name);
}
```

Output:

```text
Ashish
```

This demonstrates how **Attributes and Reflection work together**.

Attributes store metadata.

Reflection can inspect and read that metadata at runtime.

---

# 4. Records

## What is a Record?

A `record` is a C# type designed especially for data-centric models.

Records provide value-based equality by default.

Example:

```csharp
public record Employee(
    int Id,
    string Name);
```

Create objects:

```csharp
var employee1 = new Employee(
    1,
    "Ashish");

var employee2 = new Employee(
    1,
    "Ashish");
```

For records:

```csharp
Console.WriteLine(employee1 == employee2);
```

Output:

```text
True
```

The records contain the same values, so they compare as equal.

---

# 5. Record vs Class

A normal class generally uses reference identity for equality unless equality is explicitly overridden.

Records are designed to provide value-based equality.

| Class                                         | Record                                     |
| --------------------------------------------- | ------------------------------------------ |
| General-purpose reference type                | Data-oriented type                         |
| Equality commonly based on reference identity | Value-based equality by default            |
| Can be mutable                                | Often used with immutable-style properties |
| `with` expression not built in                | Supports `with` expressions                |
| Good for entities/behavior                    | Good for data/value models                 |

The choice depends on the role of the type in the application.

---

# 6. Positional Records

A positional record provides a concise way to define properties.

```csharp
public record Employee(
    int Id,
    string Name,
    string Department);
```

Create an object:

```csharp
var employee = new Employee(
    1,
    "Ashish",
    "IT");
```

Access values:

```csharp
Console.WriteLine(employee.Name);
```

---

# 7. Non-Destructive Mutation with `with`

Records support the `with` expression.

It creates a new record with selected values changed.

```csharp
var employee = new Employee(
    1,
    "Ashish",
    "IT");

var updatedEmployee = employee with
{
    Department = "Development"
};
```

The original record remains unchanged.

```text
employee
   ↓
Department = IT

        with

   ↓

updatedEmployee
   ↓
Department = Development
```

This is useful when working with immutable-style data.

---

# 8. Record Struct

C# also supports `record struct`.

```csharp
public record struct Point(
    int X,
    int Y);
```

Example:

```csharp
var point1 = new Point(10, 20);
var point2 = new Point(10, 20);

Console.WriteLine(point1 == point2);
```

Output:

```text
True
```

A `record struct` is a value type, while a `record` is normally a reference type.

---

# 9. Init-Only Properties

Records are often used with `init` properties.

```csharp
public record Employee
{
    public int Id { get; init; }

    public string Name { get; init; }
}
```

Object creation:

```csharp
var employee = new Employee
{
    Id = 1,
    Name = "Ashish"
};
```

After initialization, the properties cannot normally be assigned again.

```csharp
// employee.Name = "Rahul"; 
// Not allowed after initialization
```

This supports immutable-style object design.

---

# 10. Records and Equality

Consider two classes:

```csharp
public class EmployeeClass
{
    public int Id { get; set; }
}
```

```csharp
public record EmployeeRecord(int Id);
```

For records:

```csharp
var employee1 = new EmployeeRecord(1);
var employee2 = new EmployeeRecord(1);

Console.WriteLine(employee1 == employee2);
```

Result:

```text
True
```

Records automatically provide value-based equality semantics.

---

# 11. Records and `with`

The `with` expression is especially useful when we want a modified copy.

```csharp
public record Employee(
    int Id,
    string Name,
    string Department);

var employee1 = new Employee(
    1,
    "Ashish",
    "IT");

var employee2 = employee1 with
{
    Department = "HR"
};
```

Now:

```text
employee1.Department → IT
employee2.Department → HR
```

The original record is not modified by the `with` expression.

---

# 12. Reflection + Attributes + Records

These three concepts can work together.

Example:

```csharp
[Author("Ashish")]
public record Employee(
    int Id,
    string Name);
```

Reflection can inspect the record:

```csharp
Type type = typeof(Employee);
```

And retrieve the attribute:

```csharp
var attribute = type
    .GetCustomAttributes(typeof(AuthorAttribute), true)
    .FirstOrDefault();
```

This demonstrates a common .NET concept:

```text
Attribute
    ↓
Stores metadata

Reflection
    ↓
Reads metadata

Record
    ↓
Represents data
```

---

# 13. Practical Use Cases

### Reflection

Useful for:

* Framework development
* Plugin systems
* Runtime type inspection
* Serialization
* Dependency Injection
* Testing frameworks

### Attributes

Useful for:

* Validation metadata
* Routing
* Authorization
* Serialization configuration
* Documentation
* Custom application metadata

### Records

Useful for:

* DTOs
* API request/response models
* Configuration data
* Value objects
* Immutable-style data models

---

# Interview Questions

### 1. What is Reflection?

Reflection allows an application to inspect type information and metadata at runtime.

### 2. What is the difference between `typeof()` and `GetType()`?

`typeof()` gets type information from a type known at compile time.

```csharp
typeof(Employee)
```

`GetType()` gets the runtime type of an object.

```csharp
employee.GetType()
```

### 3. What are Attributes?

Attributes are metadata attached to program elements such as classes, methods, and properties.

### 4. How do you create a custom Attribute?

Create a class that inherits from `System.Attribute`.

```csharp
public class AuthorAttribute : Attribute
{
}
```

### 5. How can Reflection read an Attribute?

Reflection APIs such as `GetCustomAttributes()` can retrieve attributes from a type or member.

### 6. What is a Record?

A record is a C# type designed for data-centric scenarios and provides value-based equality.

### 7. What is the difference between a class and a record?

A class is a general-purpose type, while a record is designed around value-based equality and data-oriented modeling.

### 8. What is the `with` expression?

`with` creates a copy of a record with selected values changed.

### 9. What is a record struct?

A `record struct` is a value-type record that provides record-style value equality and other record features.

### 10. When should you avoid Reflection?

Reflection should be avoided when direct, compile-time code is sufficient, especially in performance-sensitive paths where its overhead is unnecessary.

---

# Key Takeaway

**Reflection** → Inspect types at runtime.

**Attributes** → Add metadata to code.

**Records** → Model data with value-based equality and convenient immutable-style patterns.

Understanding these features helps when working with modern .NET frameworks and libraries.


