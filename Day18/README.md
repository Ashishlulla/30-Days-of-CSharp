# 📘 Day 18 — Generics in C#

**Generics** allow us to write reusable code that works with different data types while maintaining compile-time type safety.

Instead of writing separate code for `int`, `string`, or other types, we can use a type parameter such as `T`.

## 🧩 Why Do We Use Generics?

Generics help us:

* Write reusable and flexible code.
* Achieve compile-time type safety.
* Reduce code duplication.
* Avoid unnecessary boxing and unboxing.
* Build type-safe collections and repositories.

## 1. Generic Class

A generic class uses a type parameter to work with different data types.

```csharp id="g3n3r1"
class Box<T>
{
    public T Value { get; set; }

    public Box(T value)
    {
        Value = value;
    }
}
```

### Using the Generic Class

```csharp id="g3n3r2"
Box<int> intBox = new Box<int>(100);

Box<string> stringBox = new Box<string>("Hello");

Console.WriteLine(intBox.Value);
Console.WriteLine(stringBox.Value);
```

**Output:**

```text
100
Hello
```

The same class works with both `int` and `string`.

## 2. Generic Method

A generic method can work with different types.

```csharp id="g3n3r3"
class Utility
{
    public static void PrintItem<T>(T item)
    {
        Console.WriteLine(item);
    }
}
```

Usage:

```csharp id="g3n3r4"
Utility.PrintItem<int>(10);
Utility.PrintItem<string>("C#");
```

The compiler can often infer the type automatically:

```csharp id="g3n3r5"
Utility.PrintItem(10);
Utility.PrintItem("C#");
```

## 3. Generic Collections

C# provides built-in generic collections such as:

* `List<T>`
* `Dictionary<TKey, TValue>`
* `HashSet<T>`
* `Queue<T>`
* `Stack<T>`

Example:

```csharp id="g3n3r6"
List<int> numbers = new List<int>();

numbers.Add(10);
numbers.Add(20);
numbers.Add(30);

foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```

The list accepts only integers, providing compile-time type safety.

## 4. Generic Interface

Interfaces can also use type parameters.

```csharp id="g3n3r7"
interface IRepository<T>
{
    void Add(T item);

    T GetById(int id);
}
```

A class can implement the interface for a specific type.

```csharp id="g3n3r8"
class ProductRepository : IRepository<string>
{
    public void Add(string item)
    {
        Console.WriteLine("Added: " + item);
    }

    public string GetById(int id)
    {
        return "Product " + id;
    }
}
```

## 5. Generic Constraints

Generic constraints restrict which types can be used as type arguments.

They are specified using the `where` keyword.

### Common Constraints

| Constraint             | Meaning                                        |
| ---------------------- | ---------------------------------------------- |
| `where T : class`      | T must be a reference type                     |
| `where T : struct`     | T must be a non-nullable value type            |
| `where T : new()`      | T must have a public parameterless constructor |
| `where T : BaseClass`  | T must derive from BaseClass                   |
| `where T : IInterface` | T must implement the interface                 |

### Example

```csharp id="g3n3r9"
class Repository<T> where T : class
{
    public void Add(T item)
    {
        Console.WriteLine("Item added.");
    }
}
```

Here, `T` must be a reference type.

## 6. Generic vs Non-Generic

| Feature     | Generics                       | Non-Generics               |
| ----------- | ------------------------------ | -------------------------- |
| Type safety | Compile-time                   | May require runtime checks |
| Code reuse  | High                           | Often limited              |
| Casting     | Usually unnecessary            | May be required            |
| Value types | Can avoid boxing in many cases | May involve boxing         |
| Flexibility | Works with different types     | Often less type-safe       |

## 7. Real-World Example — Generic Repository

In a .NET application, a generic repository can provide reusable data-access operations for different entities.

```csharp id="g3n3r0"
interface IRepository<T>
{
    void Add(T entity);

    T GetById(int id);
}
```

The same interface can be used for different entities:

```csharp id="g3n3r11"
IRepository<Product> productRepository;

IRepository<Doctor> doctorRepository;
```

This helps reduce repeated code and supports reusable architecture.

## 🧠 Interview Questions

### Q1. What are Generics in C#?

Generics allow us to write reusable, type-safe code that works with different data types.

### Q2. What is `T` in Generics?

`T` is a type parameter representing the actual type supplied when using the generic class or method.

### Q3. What are the benefits of Generics?

Type safety, code reuse, flexibility, and reduced unnecessary boxing and unboxing.

### Q4. Can a generic class have multiple type parameters?

**Yes.**

```csharp
class Pair<TKey, TValue>
{
    public TKey Key { get; set; }

    public TValue Value { get; set; }
}
```

### Q5. What is a generic constraint?

A generic constraint restricts the types that can be used as type arguments.

### Q6. Why are Generics better than using `object`?

Generics provide stronger compile-time type checking and can avoid casting and boxing in common scenarios.

## ⭐ Key Takeaway

> **Generics help us write reusable, type-safe, and flexible C# code that works with multiple data types without unnecessary duplication.**

---

