# Day 25 — Dependency Injection & Design Patterns in C#

## Overview

Modern .NET applications use **Dependency Injection (DI)** to manage dependencies between classes.

Design Patterns provide reusable approaches to common software design problems.

Together, these concepts help developers build applications that are:

* Loosely coupled
* Testable
* Maintainable
* Flexible
* Easier to extend

---

# Part 1 — Dependency Injection

## 1. What Is Dependency Injection?

**Dependency Injection** is a technique where a class receives the objects it depends on from an external source instead of creating those objects itself.

### Without Dependency Injection

```csharp
public class NotificationService
{
    private readonly EmailService _emailService;

    public NotificationService()
    {
        _emailService = new EmailService();
    }

    public void Notify(string message)
    {
        _emailService.Send(message);
    }
}
```

Here, `NotificationService` directly creates `EmailService`.

This creates tight coupling.

---

## 2. With Dependency Injection

First, create an abstraction:

```csharp
public interface IEmailService
{
    void Send(string message);
}
```

Implement the interface:

```csharp
public class EmailService : IEmailService
{
    public void Send(string message)
    {
        Console.WriteLine($"Email sent: {message}");
    }
}
```

Inject the dependency:

```csharp
public class NotificationService
{
    private readonly IEmailService _emailService;

    public NotificationService(IEmailService emailService)
    {
        _emailService = emailService;
    }

    public void Notify(string message)
    {
        _emailService.Send(message);
    }
}
```

Now `NotificationService` depends on `IEmailService`, not directly on `EmailService`.

---

# 3. Dependency Injection and IoC

DI is commonly associated with **Inversion of Control (IoC)**.

Normally, a class controls the creation of its dependencies.

With DI, that responsibility is moved outside the class.

```text
Without DI:

NotificationService
        ↓
creates
        ↓
EmailService
```

With DI:

```text
DI Container
     ↓
IEmailService
     ↓
NotificationService
```

The class receives what it needs instead of constructing the dependency itself.

---

# 4. Types of Dependency Injection

There are three commonly discussed forms of DI.

| Type                      | Description                                    |
| ------------------------- | ---------------------------------------------- |
| Constructor Injection     | Dependency is provided through the constructor |
| Setter/Property Injection | Dependency is assigned through a property      |
| Method Injection          | Dependency is passed to a method               |

### Constructor Injection

```csharp
public NotificationService(IEmailService emailService)
{
    _emailService = emailService;
}
```

This is the most common approach in ASP.NET Core.

### Setter Injection

```csharp
public IEmailService EmailService { get; set; }
```

Useful in some scenarios where a dependency is optional or replaceable.

### Method Injection

```csharp
public void Notify(
    string message,
    IEmailService emailService)
{
    emailService.Send(message);
}
```

The dependency is provided only for that particular operation.

---

# 5. Dependency Injection in ASP.NET Core

ASP.NET Core has a built-in DI container.

Services can be registered in `Program.cs`.

```csharp
builder.Services.AddScoped<IEmailService, EmailService>();
```

Now ASP.NET Core can provide `IEmailService` wherever it is requested through constructor injection.

Example:

```csharp
public class NotificationService
{
    private readonly IEmailService _emailService;

    public NotificationService(IEmailService emailService)
    {
        _emailService = emailService;
    }
}
```

The framework resolves the dependency automatically.

---

# 6. Service Lifetimes

ASP.NET Core provides three common service lifetimes.

| Lifetime  | Description                                                       |
| --------- | ----------------------------------------------------------------- |
| Transient | A new instance is created each time the service is requested      |
| Scoped    | One instance is created within a scope, commonly one HTTP request |
| Singleton | One instance is reused for the application's lifetime             |

### Transient

```csharp
builder.Services.AddTransient<IEmailService, EmailService>();
```

A new instance is normally created each time it is requested.

### Scoped

```csharp
builder.Services.AddScoped<IEmployeeService, EmployeeService>();
```

A scoped instance is normally reused within the same request scope.

This is commonly used for application services and Entity Framework Core `DbContext`.

### Singleton

```csharp
builder.Services.AddSingleton<ICacheService, CacheService>();
```

The same instance is reused for the lifetime of the application.

Singleton services should be designed carefully because they can hold shared state.

---

# 7. Why Use Dependency Injection?

DI provides several benefits:

### Loose Coupling

Classes depend on abstractions rather than concrete implementations.

### Testability

Dependencies can be replaced with mocks or fake implementations during testing.

### Maintainability

Implementations can be changed without changing the classes that consume them.

### Flexibility

Different implementations can be registered for the same abstraction.

### SOLID

DI is commonly used to support the **Dependency Inversion Principle (DIP)**.

---

# Part 2 — Design Patterns

## 8. What Are Design Patterns?

Design Patterns are reusable approaches to commonly occurring software design problems.

They are not ready-made code libraries.

Instead, they describe a general structure or approach that can be adapted to a specific application.

Examples include:

* Factory
* Singleton
* Adapter
* Observer
* Strategy
* Repository
* Unit of Work

---

# 9. Three Main Categories

Design patterns are commonly grouped into three categories.

```text
                 Design Patterns
                       │
       ┌───────────────┼───────────────┐
       ↓               ↓               ↓
  Creational       Structural      Behavioral
       │               │               │
 Creating objects   Combining      Object interaction
                    objects
```

### Creational

Focus on object creation.

Examples:

* Singleton
* Factory
* Builder

### Structural

Focus on how classes and objects are composed.

Examples:

* Adapter
* Decorator
* Facade

### Behavioral

Focus on communication and behavior between objects.

Examples:

* Observer
* Strategy
* Command

---

# 10. Singleton Pattern

The Singleton pattern ensures that a type has a single shared instance within the intended scope.

A simplified example:

```csharp
public class Logger
{
    private static readonly Logger _instance =
        new Logger();

    private Logger()
    {
    }

    public static Logger Instance => _instance;

    public void Log(string message)
    {
        Console.WriteLine(message);
    }
}
```

Usage:

```csharp
Logger.Instance.Log("Application started");
```

### Common Uses

* Logging
* Configuration
* Shared caches

### Important Note

In ASP.NET Core, singleton behavior is usually better handled through the built-in DI container rather than manually implementing a Singleton class.

```csharp
builder.Services.AddSingleton<ILoggerService, LoggerService>();
```

---

# 11. Factory Pattern

The Factory pattern separates object creation from the code that uses the object.

Example:

```csharp
public interface IShape
{
    void Draw();
}
```

Implementations:

```csharp
public class Circle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Circle");
    }
}
```

```csharp
public class Rectangle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Rectangle");
    }
}
```

Factory:

```csharp
public class ShapeFactory
{
    public IShape Create(string type)
    {
        return type switch
        {
            "circle" => new Circle(),
            "rectangle" => new Rectangle(),
            _ => throw new ArgumentException("Invalid shape")
        };
    }
}
```

The calling code does not need to know the construction details.

---

# 12. Adapter Pattern

The Adapter pattern allows incompatible interfaces to work together.

Imagine an application expects:

```csharp
public interface IPaymentService
{
    void Pay(decimal amount);
}
```

But a third-party library provides:

```csharp
public class ThirdPartyPayment
{
    public void MakePayment(decimal amount)
    {
        // Payment logic
    }
}
```

An adapter can connect them:

```csharp
public class PaymentAdapter : IPaymentService
{
    private readonly ThirdPartyPayment _payment;

    public PaymentAdapter(ThirdPartyPayment payment)
    {
        _payment = payment;
    }

    public void Pay(decimal amount)
    {
        _payment.MakePayment(amount);
    }
}
```

Now the application can work with `IPaymentService`.

---

# 13. Observer Pattern

The Observer pattern establishes a relationship where observers are notified when something changes.

A simplified interface:

```csharp
public interface IObserver
{
    void Update(string message);
}
```

An observer implementation:

```csharp
public class NotificationObserver : IObserver
{
    public void Update(string message)
    {
        Console.WriteLine(
            $"Notification: {message}");
    }
}
```

This pattern is useful for scenarios such as:

* Notifications
* Event-driven systems
* UI updates
* Monitoring changes

C# events and delegates can also be used to implement observer-style behavior.

---

# 14. Common Patterns in ASP.NET Core

| Pattern / Concept    | Common Usage                         |
| -------------------- | ------------------------------------ |
| Dependency Injection | Managing dependencies                |
| Repository           | Data access abstraction              |
| Unit of Work         | Managing related database operations |
| Factory              | Creating objects/services            |
| Strategy             | Switching between algorithms         |
| Observer             | Notifications and events             |
| Adapter              | Integrating incompatible APIs        |
| Singleton            | Shared application-level services    |

Not every application needs every pattern.

Patterns should be introduced when they solve an actual design problem.

---

# 15. DI + Design Patterns

These concepts often work together.

For example:

```text
Controller
    ↓
Service
    ↓
Interface
    ↓
Implementation
    ↓
Repository
    ↓
Database
```

ASP.NET Core's DI container can create and connect these objects automatically.

Example:

```csharp
builder.Services.AddScoped<IEmployeeService, EmployeeService>();

builder.Services.AddScoped<IEmployeeRepository,
                           EmployeeRepository>();
```

Then:

```csharp
public class EmployeeService
{
    private readonly IEmployeeRepository _repository;

    public EmployeeService(
        IEmployeeRepository repository)
    {
        _repository = repository;
    }
}
```

This produces a loosely coupled design.

---

# 16. Dependency Injection vs Design Patterns

| Dependency Injection                 | Design Patterns                                      |
| ------------------------------------ | ---------------------------------------------------- |
| Technique for providing dependencies | General design solutions                             |
| Reduces coupling                     | Address recurring design problems                    |
| Commonly built into ASP.NET Core     | Can be implemented manually or with frameworks       |
| Supports DIP                         | Includes patterns such as Factory, Adapter, Observer |

DI itself is often described as a technique rather than one of the classic GoF design patterns.

---

# Interview Questions

### 1. What is Dependency Injection?

Dependency Injection is a technique where a class receives its dependencies from outside instead of creating them internally.

### 2. What is Inversion of Control?

IoC means that control over creating and managing dependencies is moved away from the dependent class to an external mechanism such as a DI container.

### 3. What are the types of Dependency Injection?

The commonly discussed types are:

* Constructor Injection
* Setter/Property Injection
* Method Injection

### 4. What are the service lifetimes in ASP.NET Core?

* Transient
* Scoped
* Singleton

### 5. Which DI type is most commonly used in ASP.NET Core?

Constructor injection is the most commonly used approach.

### 6. What is a Design Pattern?

A design pattern is a reusable approach to solving a commonly occurring software design problem.

### 7. What are the three categories of Design Patterns?

* Creational
* Structural
* Behavioral

### 8. What is the Factory Pattern?

The Factory pattern separates object creation from the code that uses the created object.

### 9. What is the Adapter Pattern?

The Adapter pattern allows two incompatible interfaces to work together.

### 10. What is the Observer Pattern?

The Observer pattern allows interested objects to be notified when the state or behavior of another object changes.

### 11. What is the difference between Singleton and Singleton DI lifetime?

A Singleton design pattern is a design approach for controlling instance creation.

`AddSingleton()` is an ASP.NET Core DI lifetime that tells the DI container to reuse a service instance within the application's service-provider lifetime.

---

# Key Takeaway

**Dependency Injection reduces coupling by providing dependencies from outside a class.**

**Design Patterns provide reusable approaches to recurring software design problems.**

Understanding both concepts helps developers build C# and ASP.NET Core applications that are easier to test, maintain, and extend.

