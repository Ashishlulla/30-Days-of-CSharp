# 📘 Day 16 — Static Members in C#

**Static members** belong to the class itself rather than to a specific object.

They are shared across instances of the class and can be accessed using the class name.

## ⚡ Why Do We Use Static Members?

Static members help us:

* Share common data across objects.
* Create utility and helper methods.
* Avoid unnecessary object creation.
* Maintain class-level functionality.
* Store information common to all instances.

## 1. Static Field

A static field has one shared storage location for the type.

```csharp
class Student
{
    public static int TotalStudents = 0;

    public Student()
    {
        TotalStudents++;
    }
}
```

Using the static field:

```csharp
Student s1 = new Student();
Student s2 = new Student();

Console.WriteLine(Student.TotalStudents);
```

**Output:**

```text
2
```

Both objects share the same `TotalStudents` field.

## 2. Static Method

A static method belongs to the class and can be called without creating an object.

```csharp
class MathHelper
{
    public static int Add(int a, int b)
    {
        return a + b;
    }
}
```

Calling the method:

```csharp
int result = MathHelper.Add(5, 3);

Console.WriteLine(result);
```

**Output:**

```text
8
```

### Important

A static method cannot directly access instance members because it does not have an instance reference.

## 3. Static Constructor

A static constructor initializes static members.

```csharp
class Config
{
    public static string AppName;

    static Config()
    {
        AppName = "DoctorsHub";
    }
}
```

A static constructor:

* Has no parameters.
* Has no access modifier.
* Runs automatically before the type is first used in a way that requires static initialization.
* Runs at most once per type, per application domain.

## 4. Static Class

A static class can contain only static members.

```csharp
static class Utility
{
    public static void ShowMessage()
    {
        Console.WriteLine("Hello from Utility");
    }
}
```

Usage:

```csharp
Utility.ShowMessage();
```

A static class cannot be instantiated or inherited.

## 5. Static vs Instance Members

| Feature               | Static Member                   | Instance Member                            |
| --------------------- | ------------------------------- | ------------------------------------------ |
| Belongs to            | Class/type                      | Object                                     |
| Accessed using        | Class name                      | Object reference                           |
| Shared across objects | Yes                             | No, each object has its own instance state |
| Object required       | No                              | Yes, for instance access                   |
| Common use            | Shared data and utility methods | Object-specific data and behavior          |

## 6. Real-World Example — Order Counter

Suppose an application needs to generate a unique sequential order ID.

```csharp
class Order
{
    private static int orderCount = 0;

    public static int GetNextOrderId()
    {
        return ++orderCount;
    }
}
```

Usage:

```csharp
Console.WriteLine(Order.GetNextOrderId());
Console.WriteLine(Order.GetNextOrderId());
Console.WriteLine(Order.GetNextOrderId());
```

**Output:**

```text
1
2
3
```

The counter is shared by all calls because it is static.

## 7. When Should We Avoid Static?

Static members are not always the best choice.

Avoid using static state when:

* Data belongs to individual objects.
* You need different instances with independent state.
* You want easy dependency injection and unit testing.
* The functionality needs to be overridden or participate in instance-based polymorphism.

## 🧠 Interview Questions

### Q1. What are static members in C#?

Static members belong to the class rather than an individual object.

### Q2. Can a static method access instance members directly?

**No.** A static method does not have an instance reference.

### Q3. Can we create an object of a static class?

**No.** Static classes cannot be instantiated.

### Q4. Can static methods be overridden?

**No.** Static methods do not participate in runtime method overriding.

### Q5. How many times does a static constructor run?

At most once for a given type in an application domain.

### Q6. Are static fields shared among objects?

**Yes.** A static field has one shared storage location for the type.

## ⭐ Key Takeaway

> **Static members belong to the class and are useful for shared data, utility methods, and common functionality.**

---