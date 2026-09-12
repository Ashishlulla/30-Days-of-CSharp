# 📘 Day 12 — Inheritance in C#

**Inheritance** is an OOP mechanism where a **derived class** acquires properties and behavior from a **base class**.

It promotes **code reusability** and helps create a logical relationship between classes.

## 🧬 Why Do We Use Inheritance?

Inheritance helps us:

* Reuse existing code
* Reduce code duplication
* Create a logical class hierarchy
* Extend existing functionality
* Support polymorphism

## 1. Base and Derived Class

```csharp id="9j0x1p"
class Animal
{
    public void Show()
    {
        Console.WriteLine("This is an animal.");
    }
}

class Dog : Animal
{
    public void Bark()
    {
        Console.WriteLine("Dog barks.");
    }
}
```

Here:

* `Animal` → Base class
* `Dog` → Derived class
* `Dog` inherits the members of `Animal`

```csharp id="1ukq83"
Dog dog = new Dog();

dog.Show();
dog.Bark();
```

The `Dog` object can access both its own members and inherited members.

## 2. Using the `base` Keyword

The `base` keyword is used to access members of the base class.

It can be used to call a base class constructor or method.

```csharp id="y2f6ne"
class Animal
{
    public string Name;

    public Animal(string name)
    {
        Name = name;
    }
}

class Dog : Animal
{
    public Dog(string name) : base(name)
    {
    }
}
```

`base(name)` calls the constructor of the base class.

## 3. Method Overriding

A derived class can provide its own implementation of a base class method using `virtual` and `override`.

```csharp id="z6q3y8"
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal speaks.");
    }
}

class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}
```

Now:

```csharp id="d4m9x2"
Animal animal = new Dog();

animal.Speak();
```

**Output:**

```text
Dog barks.
```

This is an important example of **runtime polymorphism**.

## 4. Sealed Class

A `sealed` class cannot be inherited.

```csharp id="7zv1ka"
sealed class Cat
{
    public void Meow()
    {
        Console.WriteLine("Meow");
    }
}
```

Another class cannot inherit from `Cat`.

```csharp
// Not allowed
class PersianCat : Cat
{
}
```

## 5. Sealed Method

A method can also be marked as `sealed` to prevent further overriding.

```csharp id="x3v8qs"
class Animal
{
    public virtual void Speak()
    {
        Console.WriteLine("Animal speaks.");
    }
}

class Dog : Animal
{
    public sealed override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}
```

A further derived class cannot override `Speak()`.

## 6. Types of Inheritance

### Single Inheritance

One derived class inherits from one base class.

```text
Animal
   ↓
  Dog
```

### Multilevel Inheritance

A class derives from another derived class.

```text
Animal
   ↓
  Dog
   ↓
 Puppy
```

### Hierarchical Inheritance

Multiple classes inherit from the same base class.

```text
       Animal
       /    \
     Dog    Cat
```

### Multiple Inheritance

C# does **not** support multiple inheritance through classes.

```text
Class A ──┐
          ├──> Class C  ❌
Class B ──┘
```

However, C# supports implementing multiple interfaces.

## 🧠 Interview Questions

### Q1. Can a class inherit from multiple classes in C#?

**No.**

C# does not support multiple class inheritance.

However, a class can implement multiple interfaces.

### Q2. What is the difference between `virtual` and `override`?

`virtual` allows a base class method to be overridden.

`override` provides a new implementation of that method in the derived class.

### Q3. Can we inherit a `sealed` class?

**No.**

A sealed class cannot be inherited.

### Q4. What is the purpose of the `base` keyword?

It is used to access members of the base class, such as its constructor or methods.

### Q5. Why is inheritance useful?

It promotes **code reuse**, reduces duplication, and helps build relationships between related classes.

## ⭐ Key Takeaway

> **Inheritance helps us reuse code, build a class hierarchy, and achieve polymorphism in C#.**

