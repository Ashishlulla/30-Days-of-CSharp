# Day 23 — Collections & Advanced Generics in C#

## Overview

Collections are used to store and manage groups of objects in C#.

C# provides different collection types for different requirements, such as:

* Ordered data
* Key-value data
* Unique data
* FIFO processing
* LIFO processing
* Sorted data

Generics allow us to create reusable and type-safe classes, methods, and interfaces that can work with different data types.

---

## 1. What Are Collections?

A collection is an object used to store and manage multiple values.

For example:

```csharp
var numbers = new List<int>
{
    10, 20, 30, 40
};
```

Collections provide methods for:

* Adding data
* Removing data
* Searching data
* Sorting data
* Iterating through data

Compared with a basic array, many collection types provide more flexible operations.

---

## 2. Common Collection Types

| Collection                | Purpose                  | Example                  |
| ------------------------- | ------------------------ | ------------------------ |
| `List<T>`                 | Dynamic list of elements | `List<int>`              |
| `Array`                   | Fixed-size collection    | `int[]`                  |
| `Dictionary<TKey,TValue>` | Key-value pairs          | `Dictionary<string,int>` |
| `HashSet<T>`              | Unique elements          | `HashSet<int>`           |
| `SortedList<TKey,TValue>` | Sorted key-value pairs   | `SortedList<string,int>` |
| `Queue<T>`                | FIFO                     | `Queue<string>`          |
| `Stack<T>`                | LIFO                     | `Stack<int>`             |

---

## 3. List<T>

`List<T>` is one of the most commonly used generic collections.

```csharp
var numbers = new List<int>
{
    1, 2, 3, 4, 5
};

numbers.Add(6);
numbers.Remove(3);

foreach (var number in numbers)
{
    Console.WriteLine(number);
}
```

### Important Methods

```csharp
numbers.Add(10);
numbers.Remove(10);
numbers.Contains(5);
numbers.Clear();
numbers.Count;
```

`List<T>` is useful when the number of elements can change dynamically.

---

## 4. Array

An array has a fixed size after it is created.

```csharp
int[] numbers = { 10, 20, 30, 40 };
```

Access elements using an index:

```csharp
Console.WriteLine(numbers[0]);
```

### List vs Array

| Array                   | List<T>                   |
| ----------------------- | ------------------------- |
| Fixed size              | Dynamic size              |
| Simple and lightweight  | More collection methods   |
| Uses `Length`           | Uses `Count`              |
| Good when size is known | Good when size can change |

---

## 5. Dictionary<TKey, TValue>

A dictionary stores data as key-value pairs.

```csharp
var employees = new Dictionary<int, string>
{
    { 101, "Ashish" },
    { 102, "Rahul" },
    { 103, "Priya" }
};
```

Access a value using its key:

```csharp
string employee = employees[101];
```

Check whether a key exists:

```csharp
if (employees.ContainsKey(102))
{
    Console.WriteLine(employees[102]);
}
```

A dictionary is useful when we need fast lookup using a key.

---

## 6. HashSet<T>

`HashSet<T>` stores unique elements.

```csharp
var numbers = new HashSet<int>
{
    1, 2, 3, 3, 4
};
```

The duplicate `3` is stored only once.

```csharp
Console.WriteLine(numbers.Count);
```

`HashSet<T>` is useful when uniqueness is important.

Example:

```csharp
var skills = new HashSet<string>
{
    "C#",
    ".NET",
    "SQL",
    "C#"
};
```

Result:

```text
C#
.NET
SQL
```

---

## 7. Queue<T>

A queue follows **FIFO**:

> First In, First Out

```csharp
var queue = new Queue<string>();

queue.Enqueue("Task 1");
queue.Enqueue("Task 2");
queue.Enqueue("Task 3");

Console.WriteLine(queue.Dequeue());
```

Output:

```text
Task 1
```

Common methods:

```csharp
queue.Enqueue(item);
queue.Dequeue();
queue.Peek();
```

A queue can be useful for processing tasks in the order they were received.

---

## 8. Stack<T>

A stack follows **LIFO**:

> Last In, First Out

```csharp
var stack = new Stack<int>();

stack.Push(10);
stack.Push(20);
stack.Push(30);

Console.WriteLine(stack.Pop());
```

Output:

```text
30
```

Common methods:

```csharp
stack.Push(item);
stack.Pop();
stack.Peek();
```

Stacks are commonly used in scenarios such as undo operations and recursive processing.

---

## 9. Common Collection Interfaces

C# provides interfaces that describe collection behavior.

| Interface                  | Purpose                          |
| -------------------------- | -------------------------------- |
| `IEnumerable<T>`           | Supports enumeration             |
| `ICollection<T>`           | Adds basic collection operations |
| `IList<T>`                 | Provides index-based access      |
| `IDictionary<TKey,TValue>` | Represents key-value collections |

### Interface Relationship

A common hierarchy is:

```text
IEnumerable<T>
      ↓
ICollection<T>
      ↓
IList<T>
      ↓
List<T>
```

`IEnumerable<T>` is particularly important because it represents something that can be iterated using `foreach`.

---

## 10. Generic Collections

Generic collections provide compile-time type safety.

Example:

```csharp
List<int> numbers = new List<int>();

numbers.Add(10);
numbers.Add(20);
```

The compiler prevents adding an incompatible type:

```csharp
// numbers.Add("Hello"); // Compile-time error
```

### Benefits

* Type safety
* Better readability
* Less casting
* Reduced boxing/unboxing for value types
* Reusable code
* Better compile-time checking

---

# Advanced Generics

## 11. What Are Generics?

Generics allow us to write classes, methods, interfaces, and delegates that work with different data types.

Instead of writing:

```csharp
int GetValue()
{
    return 10;
}
```

and another method for strings, we can create reusable generic code.

```csharp
T GetValue<T>(T value)
{
    return value;
}
```

---

## 12. Generic Methods

A generic method uses a type parameter.

```csharp
public static T GetFirstItem<T>(List<T> list)
{
    return list[0];
}
```

Usage:

```csharp
var names = new List<string>
{
    "Ashish",
    "Rahul"
};

string firstName = GetFirstItem(names);
```

The same method can work with integers:

```csharp
var numbers = new List<int>
{
    10, 20, 30
};

int firstNumber = GetFirstItem(numbers);
```

The method is reusable without duplicating code.

---

## 13. Generic Classes

Generics can also be used with classes.

```csharp
public class Box<T>
{
    public T Value { get; set; }

    public Box(T value)
    {
        Value = value;
    }
}
```

Usage:

```csharp
var intBox = new Box<int>(100);

var stringBox = new Box<string>("Hello");
```

The same class works with different types.

---

## 14. Generic Constraints

Generic constraints restrict which types can be used with a generic type parameter.

### `class` Constraint

```csharp
public class Repository<T>
    where T : class
{
}
```

`T` must be a reference type.

### `struct` Constraint

```csharp
public class Calculator<T>
    where T : struct
{
}
```

`T` must be a value type.

### `new()` Constraint

```csharp
public class Factory<T>
    where T : new()
{
    public T Create()
    {
        return new T();
    }
}
```

`T` must have a public parameterless constructor.

### Multiple Constraints

```csharp
public class Repository<T>
    where T : class, new()
{
    public T Create()
    {
        return new T();
    }
}
```

Constraints help make generic code safer and more predictable.

---

## 15. Generic Interfaces

Generics can be used with interfaces.

```csharp
public interface IRepository<T>
    where T : class
{
    Task<IEnumerable<T>> GetAllAsync();

    Task<T?> GetByIdAsync(int id);
}
```

A concrete implementation can then work with a specific entity:

```csharp
public class DoctorRepository
    : IRepository<Doctor>
{
}
```

This is commonly seen in reusable application architecture.

---

## 16. Covariance and Contravariance

Variance allows certain generic types to be assigned to other compatible generic types when inheritance is involved.

There are two important keywords:

* `out` → Covariance
* `in` → Contravariance

### Covariance — `out`

Covariance allows a more derived type to be used where a base type is expected in supported generic interfaces/delegates.

```csharp
public interface IProducer<out T>
{
    T GetData();
}
```

A producer only outputs `T`.

### Contravariance — `in`

Contravariance works in the opposite direction and is useful for types that consume values.

```csharp
public interface IConsumer<in T>
{
    void Consume(T item);
}
```

A consumer receives `T` as input.

### Easy Way to Remember

```text
out → Produces
in  → Consumes
```

---

## 17. Generic Repository Example

Generics are useful for creating reusable repository abstractions.

```csharp
public interface IRepository<T>
    where T : class
{
    Task<IEnumerable<T>> GetAllAsync();

    Task<T?> GetByIdAsync(int id);
}
```

A generic repository can then be implemented for different entities.

```csharp
public class Repository<T> : IRepository<T>
    where T : class
{
    private readonly DbContext _context;

    public Repository(DbContext context)
    {
        _context = context;
    }

    public async Task<IEnumerable<T>> GetAllAsync()
    {
        return await _context.Set<T>().ToListAsync();
    }

    public async Task<T?> GetByIdAsync(int id)
    {
        return await _context.Set<T>().FindAsync(id);
    }
}
```

This demonstrates how generics can reduce duplicated repository code.

---

## 18. When to Choose Which Collection?

| Requirement           | Recommended Collection    |
| --------------------- | ------------------------- |
| Dynamic ordered data  | `List<T>`                 |
| Fixed-size data       | Array                     |
| Key-based lookup      | `Dictionary<TKey,TValue>` |
| Unique values         | `HashSet<T>`              |
| FIFO processing       | `Queue<T>`                |
| LIFO processing       | `Stack<T>`                |
| Sorted key-value data | `SortedList<TKey,TValue>` |

The correct collection depends on the operations your application performs most frequently.

---

## Interview Questions

### 1. What is the difference between an Array and List<T>?

An array has a fixed size, while `List<T>` can dynamically grow or shrink.

### 2. What is the difference between IEnumerable<T> and IList<T>?

`IEnumerable<T>` primarily provides enumeration, while `IList<T>` provides additional list operations such as index-based access and modification.

### 3. What is a Dictionary?

`Dictionary<TKey,TValue>` stores key-value pairs and allows values to be accessed using their keys.

### 4. What is the difference between Stack and Queue?

* `Stack<T>` follows LIFO.
* `Queue<T>` follows FIFO.

### 5. Why use generic collections?

Generic collections provide type safety, reduce casting, and allow reusable code.

### 6. What are generic constraints?

Generic constraints restrict the types that can be used as generic type parameters.

Examples include:

```csharp
where T : class
where T : struct
where T : new()
```

### 7. What is covariance?

Covariance is represented by `out` and allows compatible conversions for generic types that produce values.

### 8. What is contravariance?

Contravariance is represented by `in` and allows compatible conversions for generic types that consume values.

### 9. What is the benefit of generic methods?

Generic methods allow the same method implementation to work with different data types while maintaining type safety.

### 10. Why are generics preferred over object-based code?

Generics provide compile-time type safety and reduce the need for casting and boxing/unboxing.

---

## Key Takeaway

**Collections** help us store and manage data efficiently.

**Generics** allow us to create reusable, flexible, and type-safe code.

Understanding both is important for writing clean and maintainable C# applications.
