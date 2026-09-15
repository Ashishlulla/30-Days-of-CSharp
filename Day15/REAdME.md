# 📘 Day 15 — Interfaces in C#

An **interface** is a contract that defines members a class must implement.

It helps achieve abstraction, loose coupling, and flexible software design.

## 🔌 Why Do We Use Interfaces?

Interfaces help us:

* Define a common contract for related classes.
* Achieve abstraction.
* Promote loose coupling.
* Support multiple interface implementation.
* Make code easier to test and maintain.

## 1. Defining an Interface

An interface is declared using the `interface` keyword.

```csharp
interface IShape
{
    void Draw();

    double Area();
}
```

The interface defines what a class should do, but does not provide the implementation in this example.

## 2. Implementing an Interface

A class implements an interface using the `:` symbol.

```csharp
interface IShape
{
    void Draw();
}

class Circle : IShape
{
    public void Draw()
    {
        Console.WriteLine("Drawing Circle");
    }
}
```

Using the interface:

```csharp
IShape shape = new Circle();

shape.Draw();
```

**Output:**

```text
Drawing Circle
```

## 3. Multiple Interfaces

C# allows a class to implement multiple interfaces.

```csharp
interface IPrintable
{
    void Print();
}

interface IScannable
{
    void Scan();
}

class Document : IPrintable, IScannable
{
    public void Print()
    {
        Console.WriteLine("Printing...");
    }

    public void Scan()
    {
        Console.WriteLine("Scanning...");
    }
}
```

The `Document` class implements both contracts.

## 4. Real-World Example — Notifications

Different notification services can implement the same interface.

```csharp
interface INotification
{
    void Send(string message);
}

class EmailNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine("Sending Email: " + message);
    }
}

class SmsNotification : INotification
{
    public void Send(string message)
    {
        Console.WriteLine("Sending SMS: " + message);
    }
}
```

Both classes follow the same contract but provide different implementations.

## 5. Interface vs Abstract Class

| Feature              | Interface                                     | Abstract Class                       |
| -------------------- | --------------------------------------------- | ------------------------------------ |
| Main purpose         | Define a contract                             | Share behavior and define a contract |
| Implementation       | Can have default implementations in modern C# | Can contain implemented methods      |
| Instance fields      | Not allowed                                   | Allowed                              |
| Constructors         | Not allowed                                   | Allowed                              |
| Multiple inheritance | Multiple interfaces supported                 | One base class                       |
| Keyword              | `interface`                                   | `abstract`                           |

## 6. Interface vs Class

| Interface                       | Class                                 |
| ------------------------------- | ------------------------------------- |
| Defines a contract              | Defines data and behavior             |
| Cannot be instantiated directly | Can be instantiated if concrete       |
| Implemented by classes          | Can inherit from a base class         |
| Promotes loose coupling         | Represents objects and their behavior |

## 🧠 Interview Questions

### Q1. What is an interface in C#?

An interface defines a contract that specifies members a class must implement.

### Q2. Can a class implement multiple interfaces?

**Yes.** A class can implement multiple interfaces.

### Q3. Can we create an object of an interface?

**No.** An interface cannot be instantiated directly.

### Q4. What is the difference between an interface and an abstract class?

An interface primarily defines a contract, while an abstract class can provide both a contract and shared implementation.

### Q5. Can an interface contain fields or constructors?

Traditional interfaces cannot contain instance fields or constructors. Modern C# interfaces can also include features such as default method implementations and static members.

## ⭐ Key Takeaway

> **Interfaces help us achieve abstraction, define contracts, and build flexible, maintainable C# applications.**

---
