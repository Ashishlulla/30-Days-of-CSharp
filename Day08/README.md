# 📚 Day 8 — Collections in C#

Collections are used to store and manage groups of objects efficiently.

In this lesson, I explored three commonly used C# collections:

* `List<T>`
* `Dictionary<TKey, TValue>`
* `HashSet<T>`

## 🔹 List<T>

Use `List<T>` when you need **ordered data** and index-based access.

```csharp
var users = new List<string>
{
    "Ashish",
    "Rahul",
    "Priya"
};

Console.WriteLine(users[0]);
```

**Best for:** Ordered collections and index-based access.

---

## 🔹 Dictionary<TKey, TValue>

Use `Dictionary<TKey, TValue>` when you need to store data as **key-value pairs**.

```csharp
var employees = new Dictionary<int, string>
{
    { 101, "Ashish" },
    { 102, "Rahul" }
};

Console.WriteLine(employees[101]);
```

**Best for:** Fast lookup using a unique key.

---

## 🔹 HashSet<T>

Use `HashSet<T>` when you need **unique values**.

```csharp
var skills = new HashSet<string>
{
    "C#",
    "ASP.NET Core",
    "C#",
    "SQL"
};
```

Duplicate values are automatically ignored.

**Best for:** Maintaining a collection of unique values.

---

## ⚡ Performance — Big O

| Operation                        | Average Complexity |
| -------------------------------- | -----------------: |
| `List<T>.Contains()`             |               O(n) |
| `Dictionary<TKey,TValue>` lookup |               O(1) |
| `HashSet<T>.Contains()`          |               O(1) |

> ⚠️ These are typical/average complexities; actual performance depends on the operation and implementation details.

## 🧠 Quick Decision Guide

**Need order + index?** → `List<T>`

**Need key-based lookup?** → `Dictionary<TKey, TValue>`

**Need unique values?** → `HashSet<T>`

### 🎯 Key Takeaway

Don't automatically use `List<T>` for everything.

**Choose the collection based on the requirement.**

> Right collection → Better performance → Cleaner code 🚀

---

### 📌 Day 8 of 30 Days of C#

**Topic:** Collections
**Focus:** `List<T>` vs `Dictionary<TKey,TValue>` vs `HashSet<T>`

#CSharp #DotNet #Programming #100DaysOfCode #LearnCSharp
