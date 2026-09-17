# 📘 Day 17 — Enums & Structs in C#

Enums and structs are important C# concepts that help us represent related values and group data efficiently.

* **Enum:** Defines a set of named constants.
* **Struct:** A value type that groups related data and behavior.

## 1. What is an Enum?

An enum, short for enumeration, is a value type that defines a set of named constants.

It improves code readability by replacing unclear numeric values with meaningful names.

### Why Do We Use Enums?

* Improve code readability.
* Reduce magic numbers.
* Represent a fixed set of related values.
* Make code easier to maintain.
* Work well with `switch` statements.

## 2. Enum Example

```csharp
enum Day
{
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
}
```

Using the enum:

```csharp
Day today = Day.Monday;

Console.WriteLine(today);
Console.WriteLine((int)today);
```

**Output:**

```text
Monday
0
```

By default, enum members start at `0` and increase by `1`.

The default underlying type of an enum is `int`.

## 3. Custom Values in Enum

We can assign specific numeric values to enum members.

```csharp
enum Status
{
    Pending = 0,
    Approved = 1,
    Rejected = 2
}
```

Usage:

```csharp
Status status = Status.Approved;

Console.WriteLine((int)status);
```

**Output:**

```text
1
```

## 4. Enum with Switch

Enums work well with `switch` statements.

```csharp
Day day = Day.Wednesday;

switch (day)
{
    case Day.Monday:
    case Day.Tuesday:
    case Day.Wednesday:
    case Day.Thursday:
    case Day.Friday:
        Console.WriteLine("Weekday");
        break;

    case Day.Saturday:
    case Day.Sunday:
        Console.WriteLine("Weekend");
        break;
}
```

**Output:**

```text
Weekday
```

---

## 5. What is a Struct?

A struct is a **value type** used to group related data and behavior.

Structs can contain:

* Fields
* Properties
* Methods
* Constructors
* Events
* Operators

They are useful for small data structures such as coordinates, measurements, and simple values.

## 6. Struct Example

```csharp
struct Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public Point(int x, int y)
    {
        X = x;
        Y = y;
    }

    public void Display()
    {
        Console.WriteLine($"({X}, {Y})");
    }
}
```

Using the struct:

```csharp
Point point = new Point(10, 20);

point.Display();
```

**Output:**

```text
(10, 20)
```

## 7. Structs are Value Types

When a struct is assigned to another variable, its value is copied.

```csharp
Point p1 = new Point(10, 20);

Point p2 = p1;

p2.X = 100;

Console.WriteLine(p1.X);
Console.WriteLine(p2.X);
```

**Output:**

```text
10
100
```

Changing `p2` does not change `p1` because they contain separate values.

## 8. Struct vs Class

| Feature       | Struct                                      | Class                        |
| ------------- | ------------------------------------------- | ---------------------------- |
| Type          | Value type                                  | Reference type               |
| Assignment    | Copies the value                            | Copies the reference         |
| Default value | Fields are zero-initialized                 | Reference is `null`          |
| Inheritance   | Cannot inherit from another class or struct | Supports class inheritance   |
| Interfaces    | Can implement interfaces                    | Can implement interfaces     |
| Best use      | Small, simple data values                   | Complex objects and behavior |

### Important Note

Structs are not guaranteed to always be stored on the stack. Their storage depends on context, and they can be allocated on the heap when contained in objects or boxed.

## 9. When to Use Enum vs Struct?

### Use Enum When:

* You have a fixed set of related values.
* You want meaningful names instead of numbers.
* You need to represent states, categories, or options.

**Examples:** `Status`, `Day`, `AppointmentType`.

### Use Struct When:

* You need to group related data.
* The type represents a small value.
* Value-copy behavior is appropriate.
* You want a lightweight data type.

**Examples:** `Point`, `DateTime`, `TimeSpan`.

## 🧠 Interview Questions

### Q1. What is an enum in C#?

An enum is a value type that defines a set of named constants.

### Q2. What is the default underlying type of an enum?

`int`.

### Q3. Is a struct a value type or reference type?

A struct is a value type.

### Q4. Can a struct inherit from another class?

No. Structs cannot inherit from another class or struct, but they can implement interfaces.

### Q5. Can a struct have constructors and methods?

Yes. Structs can have constructors, methods, properties, and other members.

### Q6. What happens when a struct is assigned to another variable?

Its value is copied. The two variables then contain independent values.

## ⭐ Key Takeaway

> **Enums give meaningful names to related values, while structs help group data efficiently as value types.**

---
