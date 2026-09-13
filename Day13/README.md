# 📘 Day 13 — Polymorphism in C#

**Polymorphism** is an OOP concept that allows the same method or interface to behave in different ways.

The word polymorphism means **"many forms."**

It makes code more flexible, reusable, and easier to extend.

## 🎭 Why Do We Use Polymorphism?

Polymorphism helps us:

* Write flexible code
* Reduce code duplication
* Make applications easier to extend
* Support different implementations through a common interface
* Achieve runtime behavior using inheritance

## 1. Types of Polymorphism

C# mainly supports two forms of polymorphism:

```text
              Polymorphism
                  /    \
                 /      \
        Compile-time    Runtime
           (Static)     (Dynamic)
              |             |
        Overloading      Overriding
```

### Compile-Time Polymorphism

Resolved by the compiler.

Common examples:

* Method overloading
* Operator overloading

### Runtime Polymorphism

Resolved while the program is running.

Common example:

* Method overriding

---

## 2. Method Overloading

Method overloading means having multiple methods with the **same name but different parameter lists**.

```csharp
class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
}
```

The compiler determines which method to call based on the arguments.

```csharp
Calculator calculator = new Calculator();

Console.WriteLine(calculator.Add(10, 20));
Console.WriteLine(calculator.Add(10, 20, 30));
```

### Key Point

> **Same method name + different parameters = Method Overloading**

---

## 3. Operator Overloading

C# allows certain operators to be overloaded for user-defined types.

```csharp
class Complex
{
    public int Real { get; set; }
    public int Imaginary { get; set; }

    public static Complex operator +(Complex a, Complex b)
    {
        return new Complex
        {
            Real = a.Real + b.Real,
            Imaginary = a.Imaginary + b.Imaginary
        };
    }
}
```

This allows the `+` operator to work with `Complex` objects.

Operator overloading is resolved at **compile time**.

---

## 4. Method Overriding

Method overriding allows a derived class to provide its own implementation of a method defined in the base class.

The base class method uses `virtual`, while the derived class uses `override`.

```csharp
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

```csharp
Animal animal = new Dog();

animal.Speak();
```

**Output:**

```text
Dog barks.
```

The actual method that runs is determined at **runtime** based on the object.

---

## 5. Real-World Example

Consider a payment system.

Different payment methods can perform the same operation differently.

```text
             Payment
             /   |   \
            /    |    \
     Credit Card UPI  Net Banking
```

All payment types can have a common method:

```csharp
public virtual void Pay()
{
    Console.WriteLine("Processing payment...");
}
```

Each payment type can provide its own implementation.

This allows the application to work with different payment types through a common base type.

---

## 6. Overloading vs Overriding

| Feature          | Overloading               | Overriding               |
| ---------------- | ------------------------- | ------------------------ |
| Resolved         | Compile time              | Runtime                  |
| Purpose          | Different parameter lists | Different implementation |
| Classes          | Usually same class        | Base & derived classes   |
| Method signature | Must differ               | Same signature           |
| Keywords         | Not required              | `virtual` / `override`   |
| Polymorphism     | Compile-time              | Runtime                  |

### Easy way to remember

> **Overloading → Same name, different parameters**

> **Overriding → Same method, different implementation**

---

## 7. Important Keywords

### `virtual`

Allows a base class method to be overridden.

```csharp
public virtual void Speak()
{
}
```

### `override`

Provides a new implementation in the derived class.

```csharp
public override void Speak()
{
}
```

---

## 🧠 Interview Questions

### Q1. What is polymorphism?

Polymorphism allows the same method or object to behave in different ways.

### Q2. What is the difference between overloading and overriding?

**Overloading** occurs at compile time and uses different parameter lists.

**Overriding** occurs at runtime and provides a different implementation in a derived class.

### Q3. Can a static method be overridden?

**No.**

Static methods belong to the type rather than an object, so they cannot be overridden.

### Q4. Which keywords are commonly used for method overriding?

`virtual` is used in the base class and `override` is used in the derived class.

### Q5. What is runtime polymorphism?

Runtime polymorphism occurs when the method implementation is selected at runtime based on the actual object.

---

## ⭐ Key Takeaway

> **Polymorphism makes your C# code flexible, reusable, and easier to maintain.**

---

