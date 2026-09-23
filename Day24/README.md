# Day 24 — SOLID Principles in C#

## Overview

**SOLID** is a collection of five object-oriented design principles that help developers build code that is:

* Maintainable
* Flexible
* Testable
* Reusable
* Easier to extend

The five SOLID principles are:

| Letter | Principle                       |
| ------ | ------------------------------- |
| S      | Single Responsibility Principle |
| O      | Open/Closed Principle           |
| L      | Liskov Substitution Principle   |
| I      | Interface Segregation Principle |
| D      | Dependency Inversion Principle  |

SOLID principles are especially useful when designing larger applications where requirements and business rules change over time.

---

# 1. Single Responsibility Principle — SRP

### Definition

> A class should have one primary responsibility and one reason to change.

The goal is to avoid putting unrelated responsibilities into the same class.

### Example — Problem

```csharp id="5e1yqx"
public class Employee
{
    public void SaveEmployee(Employee employee)
    {
        // Save employee to database
    }

    public void PrintEmployee(Employee employee)
    {
        // Print employee details
    }

    public decimal CalculateSalary(Employee employee)
    {
        // Calculate salary
        return 0;
    }
}
```

The class is handling multiple responsibilities:

* Data persistence
* Printing
* Salary calculation

This makes the class harder to maintain and test.

### Better Approach

Separate responsibilities:

```csharp id="3s8o5r"
public class EmployeeService
{
    public void SaveEmployee(Employee employee)
    {
        // Save employee
    }
}
```

```csharp id="6s1f9z"
public class EmployeePrinter
{
    public void PrintEmployee(Employee employee)
    {
        // Print employee
    }
}
```

```csharp id="1z2x4q"
public class SalaryCalculator
{
    public decimal CalculateSalary(Employee employee)
    {
        return 0;
    }
}
```

Each class now has a focused responsibility.

### Key Idea

**One class → one primary responsibility.**

---

# 2. Open/Closed Principle — OCP

### Definition

> Software entities should be open for extension but closed for modification.

This means we should be able to add new behavior without repeatedly modifying stable existing code.

### Example

Suppose an application calculates areas of different shapes.

Instead of creating a large `if-else` or `switch` statement for every shape, we can use an abstraction.

```csharp id="p3g7v2"
public interface IShape
{
    double CalculateArea();
}
```

A circle can implement it:

```csharp id="x8m4na"
public class Circle : IShape
{
    public double Radius { get; set; }

    public double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}
```

Later, a rectangle can be added:

```csharp id="u7k2bc"
public class Rectangle : IShape
{
    public double Width { get; set; }
    public double Height { get; set; }

    public double CalculateArea()
    {
        return Width * Height;
    }
}
```

The existing `IShape` abstraction remains unchanged while new implementations can be added.

### Key Idea

**Extend behavior without unnecessarily modifying existing stable code.**

---

# 3. Liskov Substitution Principle — LSP

### Definition

> Objects of a derived type should be usable wherever objects of the base type are expected without breaking the expected behavior.

Inheritance should represent a valid behavioral relationship, not just a convenient code-sharing mechanism.

### Example

```csharp id="1w4j2e"
public class Bird
{
    public virtual void Fly()
    {
        Console.WriteLine("Bird is flying");
    }
}
```

```csharp id="6q9m3p"
public class Sparrow : Bird
{
    public override void Fly()
    {
        Console.WriteLine("Sparrow is flying");
    }
}
```

A `Sparrow` can be used where a `Bird` is expected without changing the expected behavior.

```csharp id="3d6k8a"
Bird bird = new Sparrow();

bird.Fly();
```

### Important Point

A common example is a `Penguin` inheriting from a `Bird` class that requires every bird to fly.

If the base abstraction requires behavior that a valid subtype cannot support, the abstraction may be poorly designed.

### Key Idea

**Subtypes should honor the expectations established by their base types.**

---

# 4. Interface Segregation Principle — ISP

### Definition

> Clients should not be forced to depend on interfaces they do not use.

Instead of creating one large interface, prefer smaller, focused interfaces.

### Problem

```csharp id="n5r8tc"
public interface IWorker
{
    void Work();
    void Eat();
}
```

A class that only needs `Work()` may be forced to implement `Eat()` as well.

### Better Approach

Split the interface:

```csharp id="h4q6vd"
public interface IWork
{
    void Work();
}

public interface IEat
{
    void Eat();
}
```

Now a class can implement only what it actually needs:

```csharp id="z7p2mk"
public class Human : IWork, IEat
{
    public void Work()
    {
        // Work
    }

    public void Eat()
    {
        // Eat
    }
}
```

Another class could implement only `IWork`.

### Key Idea

**Prefer small, focused interfaces over large interfaces with unrelated methods.**

---

# 5. Dependency Inversion Principle — DIP

### Definition

> High-level modules should not depend directly on low-level modules. Both should depend on abstractions.

Also:

> Abstractions should not depend on details. Details should depend on abstractions.

### Problem

```csharp id="w8c3qn"
public class EmailService
{
    public void Send(string message)
    {
        // Send email
    }
}

public class NotificationService
{
    private EmailService _emailService = new EmailService();

    public void Notify(string message)
    {
        _emailService.Send(message);
    }
}
```

`NotificationService` is tightly coupled to `EmailService`.

### Better Approach

Create an abstraction:

```csharp id="v4n7ks"
public interface INotificationService
{
    void Send(string message);
}
```

Implement the abstraction:

```csharp id="s6m2xp"
public class EmailService : INotificationService
{
    public void Send(string message)
    {
        // Send email
    }
}
```

Inject the abstraction:

```csharp id="c9r5wb"
public class NotificationService
{
    private readonly INotificationService _notificationService;

    public NotificationService(
        INotificationService notificationService)
    {
        _notificationService = notificationService;
    }

    public void Notify(string message)
    {
        _notificationService.Send(message);
    }
}
```

Now `NotificationService` depends on an abstraction rather than a concrete implementation.

This also makes unit testing easier because a mock or fake implementation can be supplied.

### Key Idea

**Depend on abstractions, not concrete implementations.**

---

# SOLID Summary

| Principle | Main Idea                                    |
| --------- | -------------------------------------------- |
| **SRP**   | One primary responsibility                   |
| **OCP**   | Open for extension, closed for modification  |
| **LSP**   | Subtypes should honor base-type expectations |
| **ISP**   | Prefer small, focused interfaces             |
| **DIP**   | Depend on abstractions                       |

---

# SOLID in ASP.NET Core

SOLID principles appear frequently in real-world .NET applications.

For example:

### Dependency Injection

ASP.NET Core has built-in dependency injection:

```csharp id="j4t8pn"
builder.Services.AddScoped<IEmailService, EmailService>();
```

A controller can depend on the interface:

```csharp id="m7q2vx"
public class NotificationController : Controller
{
    private readonly IEmailService _emailService;

    public NotificationController(
        IEmailService emailService)
    {
        _emailService = emailService;
    }
}
```

This demonstrates **Dependency Inversion** and supports loose coupling.

---

# SOLID and Clean Architecture

SOLID principles also support architectural patterns such as Clean Architecture.

For example:

```text
Controller
    ↓
Application Service
    ↓
Interface / Abstraction
    ↓
Infrastructure Implementation
```

The application layer can depend on abstractions while infrastructure provides concrete implementations.

This helps keep business logic independent from external details such as databases, email providers, or APIs.

---

# Common Mistakes

### 1. Creating interfaces for everything

SOLID does not mean every class must have an interface.

Create abstractions where they provide a meaningful design or testing benefit.

### 2. Overusing inheritance

Inheritance should represent a valid behavioral relationship.

Composition is often a better choice when objects simply need to use another object's functionality.

### 3. Making classes unnecessarily small

SRP does not mean every method needs its own class.

A class can contain multiple closely related operations while still having one cohesive responsibility.

### 4. Confusing DIP with Dependency Injection

**Dependency Inversion Principle** is a design principle.

**Dependency Injection** is a technique commonly used to implement that principle.

---

# Interview Questions

### 1. What are SOLID principles?

SOLID is a group of five object-oriented design principles used to create maintainable, flexible, and loosely coupled software.

### 2. What is SRP?

The Single Responsibility Principle states that a class should have one primary responsibility and one reason to change.

### 3. What is OCP?

The Open/Closed Principle states that software should be open for extension but closed for modification.

### 4. What is LSP?

The Liskov Substitution Principle states that derived types should be usable wherever their base types are expected without breaking expected behavior.

### 5. What is ISP?

The Interface Segregation Principle says that clients should not be forced to depend on methods they do not use.

### 6. What is DIP?

The Dependency Inversion Principle says that high-level and low-level modules should depend on abstractions rather than high-level modules directly depending on concrete implementations.

### 7. What is the difference between DIP and Dependency Injection?

**DIP** is a design principle.

**Dependency Injection** is a technique used to provide dependencies from outside a class.

### 8. How does Dependency Injection support SOLID?

Dependency Injection helps reduce tight coupling by allowing a class to depend on abstractions instead of creating concrete dependencies itself.

---

# Key Takeaway

**SOLID = Better Design + Lower Coupling + Easier Maintenance**

The goal of SOLID is not to add complexity. The goal is to design code so that responsibilities are clear, dependencies are manageable, and new requirements can be handled with less impact on existing code.

