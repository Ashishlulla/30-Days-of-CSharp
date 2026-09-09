# Day 09 — Classes & Objects

## 📌 Overview

A **class** is not an object.

Understanding the difference between classes and objects is one of the fundamental concepts of **Object-Oriented Programming (OOP)** in C#.

---

## 🎯 Learning Objectives

By the end of this day, you should understand:

* What a class is
* What an object is
* The difference between a class and an object
* How to create objects using the `new` keyword
* How objects access properties and methods
* The relationship between a class and its objects

---

## 🧱 What is a Class?

A **class** is a blueprint or template used to define the structure and behavior of objects.

A class can contain:

* Properties
* Fields
* Methods
* Constructors
* Events

Example:

```csharp
class Car
{
    public string Brand { get; set; }

    public void Drive()
    {
        Console.WriteLine("Car is driving...");
    }
}
```

The `Car` class defines what a car object can **have** and **do**.

---

## 🚗 What is an Object?

An **object** is an instance of a class.

Objects are created using the `new` keyword.

```csharp
Car car1 = new Car();

car1.Brand = "Toyota";
car1.Drive();
```

Here:

* `Car` → Class
* `car1` → Object
* `new Car()` → Creates an instance of `Car`

---

## 🔍 Class vs Object

| Class                              | Object                    |
| ---------------------------------- | ------------------------- |
| Blueprint / template               | Actual instance           |
| Defines structure                  | Contains actual data      |
| Defines behavior                   | Uses the defined behavior |
| Can contain properties and methods | Accesses those members    |
| Conceptual definition              | Runtime instance          |

---

## 🧠 Easy Way to Remember

Think about a house:

**House Blueprint → Class**

**Actual House → Object**

One class can be used to create multiple objects.

```csharp
Car car1 = new Car();
Car car2 = new Car();
Car car3 = new Car();
```

All three objects are created from the same `Car` class, but each object can have its own state.

---

## 💻 Complete Example

```csharp
class Car
{
    public string Brand { get; set; }

    public void Drive()
    {
        Console.WriteLine($"{Brand} is driving...");
    }
}

class Program
{
    static void Main()
    {
        Car car1 = new Car();

        car1.Brand = "Toyota";

        car1.Drive();
    }
}
```

### Output

```text
Toyota is driving...
```

---

## ⭐ Key Takeaway

> **A class defines what an object can have and do.
> An object is the actual instance created from that class.**

---

## 📚 What's Next?

**Day 10 — Constructors**

We'll learn how constructors initialize objects and why they are important when creating class instances.

---

### #30DaysOfCSharp #CSharp #DotNet #OOP #Programming
