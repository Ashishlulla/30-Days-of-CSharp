# Day 07 — Arrays & Strings in C#

> **Before LINQ, every C# developer should be comfortable with arrays and strings.**

Arrays and strings are fundamental C# concepts that you'll use frequently when working with collections, data processing, and LINQ.

---

## 🔹 1. Arrays

An array stores multiple values of the **same type**.

### Declaration & Initialization

```csharp
int[] numbers = { 10, 20, 30, 40 };
```

Arrays use **zero-based indexing**.

```csharp
Console.WriteLine(numbers[0]); // 10
Console.WriteLine(numbers[2]); // 30
```

### Length

The `Length` property returns the number of elements.

```csharp
Console.WriteLine(numbers.Length); // 4
```

### Looping Through Arrays

Using `for`:

```csharp
for (int i = 0; i < numbers.Length; i++)
{
    Console.WriteLine(numbers[i]);
}
```

Using `foreach`:

```csharp
foreach (int number in numbers)
{
    Console.WriteLine(number);
}
```

💡 Use `for` when you need the **index** and `foreach` when you only need the **values**.

---

## 🔹 2. Strings

A `string` represents a sequence of characters.

```csharp
string name = "Ashish";

Console.WriteLine(name);
```

Strings in C# are **immutable**, meaning operations that appear to modify a string actually return a new string.

---

## 🔥 3 Useful String Operations

### 1. `ToUpper()` / `ToLower()`

Changes the case of the string.

```csharp
string name = "Ashish";

Console.WriteLine(name.ToUpper());
// ASHISH

Console.WriteLine(name.ToLower());
// ashish
```

---

### 2. `Contains()`

Checks whether a string contains a specific value.

```csharp
string email = "ashish@example.com";

bool result = email.Contains("@");

Console.WriteLine(result);
// True
```

---

### 3. `Replace()`

Replaces one value with another.

```csharp
string message = "Hello World";

string updated = message.Replace("World", "C#");

Console.WriteLine(updated);
// Hello C#
```

---

## ⚠️ Common Mistakes

### Array index starts from `0`

```csharp
int[] numbers = { 10, 20, 30 };

Console.WriteLine(numbers[0]); // 10
```

Trying to access an invalid index can cause:

```text
IndexOutOfRangeException
```

### Remember

```text
Array → Fixed-size collection
String → Sequence of characters
Index  → Starts from 0
```

---

## 💡 Key Takeaway

> **Arrays help us store multiple values of the same type, while strings help us work with text.**

Master these fundamentals before moving deeper into **LINQ and Collections**.

### 📚 What I Learned Today

* Arrays
* Zero-based indexing
* `Length`
* `for` vs `foreach`
* Strings
* `ToUpper()` / `ToLower()`
* `Contains()`
* `Replace()`
* String immutability

**Day 07/30 — Keep learning C# 🚀**
