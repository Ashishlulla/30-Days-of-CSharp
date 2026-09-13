# 📘 Day 14 — Abstraction in C#

**Abstraction** is an OOP concept that hides implementation details and exposes only the essential features.

It helps reduce complexity and makes code easier to maintain and extend.

## 🎯 Why Do We Use Abstraction?

Abstraction helps us:

* Hide unnecessary implementation details
* Reduce complexity
* Focus on essential functionality
* Build flexible and maintainable code
* Achieve loose coupling

## 1. How Does Abstraction Work?

In C#, abstraction is commonly achieved using:

* **Abstract classes**
* **Abstract methods**
* **Interfaces**

The main idea is:

> **Define what an object should do without exposing how it does it.**

---

## 2. Abstract Class

An abstract class is declared using the `abstract` keyword.

```csharp id="7t6k2p"
abstract class Animal
{
    public abstract void Speak();

    public void Sleep()
    {
        Console.WriteLine("Sleeping...");
    }
}
```

An abstract class can contain both:

* Abstract members
* Non-abstract members

### Important

We cannot create an object directly from an abstract class.

```csharp id="5s8wqk"
// Not allowed
Animal animal = new Animal();
```

---

## 3. Abstract Method

An abstract method has **no implementation/body**.

```csharp id="1j3v9c"
abstract class Animal
{
    public abstract void Speak();
}
```

A derived class must provide the implementation.

```csharp id="r5k8fd"
class Dog : Animal
{
    public override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }
}
```

### Key Point

> **Abstract method → declaration in base class, implementation in derived class.**

---

## 4. Interface — Another Way of Abstraction

An interface defines a contract that a class agrees to implement.

```csharp id="6v2n8x"
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

The `Circle` class provides the implementation required by `IShape`.

Interfaces are useful when different classes need to follow the same contract.

---

## 5. Abstract Class vs Interface

| Abstract Class                                | Interface                                                                 |
| --------------------------------------------- | ------------------------------------------------------------------------- |
| Can contain abstract and non-abstract members | Defines a contract; can also contain default implementations in modern C# |
| Can have fields and constructors              | Cannot have instance fields or constructors                               |
| A class can inherit one base class            | A class can implement multiple interfaces                                 |
| Uses `abstract`                               | Uses `interface`                                                          |

### Easy way to remember

> **Abstract class → shared base + contract**

> **Interface → contract/capability**

---

## 6. Real-World Example — Payment System

Consider a payment system.

Different payment methods perform payment differently, but all of them follow the same basic contract.

```text
                Payment
              /    |     \
             /     |      \
      Credit Card  UPI   Net Banking
       Payment    Payment   Payment
```

We can define the contract using an abstract class:

```csharp id="2q9w4m"
abstract class Payment
{
    public abstract void Pay();
}
```

Then each payment type provides its own implementation:

```csharp id="4h7x1n"
class CreditCardPayment : Payment
{
    public override void Pay()
    {
        Console.WriteLine("Paying with Credit Card");
    }
}
```

The calling code doesn't need to know the internal payment process.

It only needs to know that the payment object can perform `Pay()`.

---

## 7. Abstraction vs Encapsulation

| Abstraction                          | Encapsulation                                  |
| ------------------------------------ | ---------------------------------------------- |
| Hides implementation details         | Hides and protects data                        |
| Uses abstract classes and interfaces | Uses access modifiers, properties, and methods |
| Focuses on essential features        | Focuses on data access and control             |
| Answers **"What to do?"**            | Answers **"How to control access?"**           |

### Simple Example

**Abstraction:**

> "A car can start."

We don't need to know every internal engine operation.

**Encapsulation:**

> The car's internal data and mechanisms are protected from direct access.

---

## 🧠 Interview Questions

### Q1. What is abstraction in C#?

Abstraction hides implementation details and exposes only the essential features of an object.

### Q2. Can we create an object of an abstract class?

**No.**

An abstract class cannot be instantiated directly.

### Q3. Can an abstract class contain non-abstract methods?

**Yes.**

An abstract class can contain both abstract and non-abstract members.

### Q4. Can an abstract method have a body?

**No.**

An abstract method only defines the required behavior. The derived class provides the implementation.

### Q5. Can a class implement multiple interfaces?

**Yes.**

A class can implement multiple interfaces even though C# does not support multiple inheritance between classes.

### Q6. What is the difference between abstraction and encapsulation?

**Abstraction** hides implementation complexity.

**Encapsulation** protects data and controls how it can be accessed or modified.

---

## ⭐ Key Takeaway

> **Abstraction helps us hide complexity, focus on essential features, and build flexible and maintainable code.**

---

