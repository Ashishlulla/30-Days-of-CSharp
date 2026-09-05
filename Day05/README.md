# 📘 Day 05 — Loops in C#

Loops are used to **execute a block of code repeatedly** based on a condition.

## 🔄 Types of Loops

### 1. `for` Loop

Best when the number of iterations is known.

```csharp
for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);
}
```

### 2. `while` Loop

The condition is checked **before** executing the loop body.

```csharp
int i = 0;

while (i < 5)
{
    Console.WriteLine(i);
    i++;
}
```

### 3. `do-while` Loop

The loop body executes **at least once** because the condition is checked afterward.

```csharp
int i = 0;

do
{
    Console.WriteLine(i);
    i++;
} while (i < 5);
```

### 4. `foreach` Loop

Best for iterating through arrays and collections.

```csharp
string[] names = { "Amit", "Vishal", "Neha" };

foreach (string name in names)
{
    Console.WriteLine(name);
}
```

## 🛑 `break` vs `continue`

| Keyword    | Purpose                     |
| ---------- | --------------------------- |
| `break`    | Completely exits the loop   |
| `continue` | Skips the current iteration |

### Example

```csharp
for (int i = 0; i < 5; i++)
{
    if (i == 2)
        continue;

    Console.WriteLine(i);
}
```

**Output:**

```text
0
1
3
4
```

## 🔁 Nested Loops

A loop inside another loop is called a **nested loop**.

```csharp
for (int i = 1; i <= 3; i++)
{
    for (int j = 1; j <= 3; j++)
    {
        Console.WriteLine($"{i} {j}");
    }
}
```

## 🧠 Which Loop Should You Use?

| Loop       | Best Used When                                    |
| ---------- | ------------------------------------------------- |
| `for`      | Number of iterations is known                     |
| `while`    | Number of iterations is unknown / condition-based |
| `do-while` | Code must execute at least once                   |
| `foreach`  | Iterating over arrays or collections              |

## 🎯 Interview Quick Questions

**Q1. What is the difference between `break` and `continue`?**

`break` terminates the loop, while `continue` skips the current iteration.

**Q2. Which loop always executes at least once?**

`do-while`.

**Q3. When is `foreach` commonly used?**

When iterating through arrays or collections.

## ⭐ Key Takeaway

> **Choose the right loop based on the requirement to write clean, readable, and efficient C# code.**

---

📅 **Part of the 30-Day C# Learning Series**

**Day 05 → Loops in C# 🔄**
