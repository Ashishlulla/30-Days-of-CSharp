# 📘 Day 10 — Constructors in C#

A **constructor** is a special member of a class used to **initialize an object**.

It is called automatically when an object is created using the `new` keyword.

## 🏗️ Basic Constructor

```csharp
class Employee
{
    public string Name;

    public Employee(string name)
    {
        Name = name;
    }
}

Employee emp = new Employee("Ashish");

Console.WriteLine(emp.Name);
```

The constructor initializes the object with the required values.

## 1. Default Constructor

A constructor with no parameters.

```csharp
class Person
{
    public Person()
    {
        Console.WriteLine("Default constructor");
    }
}
```

If no constructor is defined, C# provides a default parameterless constructor.

## 2. Parameterized Constructor

A constructor that accepts parameters.

```csharp
class Employee
{
    public string Name;

    public Employee(string name)
    {
        Name = name;
    }
}
```

Useful for initializing objects with specific values.

## 3. Constructor Overloading

A class can have multiple constructors with different parameter lists.

```csharp
class Student
{
    public Student()
    {
    }

    public Student(string name)
    {
        Name = name;
    }

    public string Name { get; set; }
}
```

## 4. Constructor Chaining

One constructor can call another constructor using `this()`.

```csharp
class Car
{
    public string Model { get; set; }

    public Car() : this("Unknown")
    {
    }

    public Car(string model)
    {
        Model = model;
    }
}
```

Constructor chaining helps avoid duplicate initialization code.

## 5. Static Constructor

A static constructor is used to initialize **static members**.

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

* Has no parameters
* Has no access modifier
* Runs automatically
* Runs only once for the type

## 6. Private Constructor

A private constructor prevents objects from being created directly from outside the class.

```csharp
class Singleton
{
    private Singleton()
    {
    }

    public static Singleton Instance { get; } = new Singleton();
}
```

Private constructors are commonly used in patterns such as **Singleton**.

## 🔄 Constructor vs Method

| Constructor             | Method                      |
| ----------------------- | --------------------------- |
| Called automatically    | Called explicitly           |
| Same name as the class  | Can have any valid name     |
| No return type          | Has a return type or `void` |
| Used for initialization | Used to perform operations  |

## 🧠 Interview Questions

### Q1. Can a constructor have a return type?

**No.** A constructor does not have a return type, not even `void`.

### Q2. Can a static constructor have parameters?

**No.** Static constructors cannot have parameters.

### Q3. What is the main purpose of a constructor?

To **initialize an object and prepare it for use**.

### Q4. Can we call a constructor explicitly?

Not like a normal method. Constructors are invoked when creating an object using `new`.

## ⭐ Key Takeaway

> **Constructors initialize objects and prepare them for use.**


