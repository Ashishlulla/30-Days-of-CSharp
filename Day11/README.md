# 📘 Day 11 — Encapsulation in C#

**Encapsulation** is the process of wrapping data and behavior inside a class and controlling access to that data.

It helps protect an object's state by restricting direct access and providing controlled access through properties or methods.

## 🔒 Why Do We Use Encapsulation?

Encapsulation helps us:

* Hide internal details of a class
* Prevent accidental modification of data
* Control how data is accessed
* Improve security and maintainability
* Keep related data and behavior together

## 1. Access Modifiers

Access modifiers define where a class member can be accessed.

| Modifier    | Access                                          |
| ----------- | ----------------------------------------------- |
| `public`    | Accessible from anywhere                        |
| `private`   | Accessible only within the class                |
| `protected` | Accessible within the class and derived classes |
| `internal`  | Accessible within the same assembly/project     |

## 2. Example — Encapsulation

```csharp
class Employee
{
    private string name;

    public string Name
    {
        get { return name; }
        set { name = value; }
    }
}
```

The `name` field is private, so it cannot be accessed directly from outside the class.

The `Name` property provides controlled access.

## 3. Properties

Properties allow us to control access to private fields using `get` and `set`.

```csharp
class Product
{
    private decimal price;

    public decimal Price
    {
        get { return price; }
        set { price = value; }
    }
}
```

We can also add validation inside a property.

```csharp
class Product
{
    private decimal price;

    public decimal Price
    {
        get { return price; }
        set
        {
            if (value >= 0)
                price = value;
        }
    }
}
```

## 4. Auto-Properties

C# provides a shorter syntax when we don't need custom logic.

```csharp
class Person
{
    public string Name { get; set; }
}
```

The compiler creates the required backing field automatically.

## 5. Real-World Example

Consider a bank account.

We don't want anyone to directly modify the account balance.

```csharp
class BankAccount
{
    private decimal balance;

    public decimal Balance
    {
        get { return balance; }
        private set { balance = value; }
    }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            Balance += amount;
    }
}
```

Here, the balance cannot be directly changed from outside the class.

Instead, controlled operations such as `Deposit()` can modify it.

## 6. Encapsulation vs Abstraction

| Encapsulation                      | Abstraction                          |
| ---------------------------------- | ------------------------------------ |
| Hides and protects data            | Hides implementation details         |
| Uses access modifiers              | Uses abstract classes/interfaces     |
| Uses private fields and properties | Uses abstract methods and interfaces |
| Focuses on data access and control | Focuses on essential behavior        |

## 🧠 Interview Questions

### Q1. Can we access a private field from outside the class?

**No.**

Private members are accessible only within the class where they are declared.

### Q2. What is the main purpose of encapsulation?

To **protect data and provide controlled access** to an object's state.

### Q3. What is the difference between a field and a property?

A **field** directly stores data, while a **property** provides controlled access to data.

### Q4. Can a property have a private setter?

**Yes.**

```csharp
public decimal Balance { get; private set; }
```

This allows the value to be read publicly but modified only inside the class.

## ⭐ Key Takeaway

> **Encapsulation protects data by restricting access and providing controlled access through properties and methods.**


