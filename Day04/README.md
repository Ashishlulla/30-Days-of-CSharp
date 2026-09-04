# 🚀 30 Days of C# — Day 4/30

📅 **Date:** 4 September 2026
📚 **Topic:** Conditions in C#

## 📌 Overview

Conditions allow a program to make decisions based on whether an expression is `true` or `false`.

C# provides `if`, `else`, `else if`, `switch`, and modern switch expressions for decision-making.

---

## 1. `if` Statement

Executes a block when the condition is `true`.

```csharp
int age = 20;

if (age >= 18)
{
    Console.WriteLine("Adult");
}
```

---

## 2. `if-else`

Executes one block when the condition is `true`, otherwise another.

```csharp
if (age >= 18)
{
    Console.WriteLine("Adult");
}
else
{
    Console.WriteLine("Minor");
}
```

---

## 3. `else if` Ladder

Used when multiple conditions need to be checked.

```csharp
if (marks >= 90)
    grade = "A";
else if (marks >= 75)
    grade = "B";
else if (marks >= 50)
    grade = "C";
else
    grade = "F";
```

Conditions are evaluated **from top to bottom**. Once a condition is satisfied, the remaining conditions are skipped.

---

## 4. Nested `if`

An `if` statement inside another `if`.

```csharp
if (isLoggedIn)
{
    if (isAdmin)
    {
        Console.WriteLine("Welcome Admin");
    }
}
```

⚠️ Avoid excessive nesting when the logic can be simplified.

---

## 5. `switch` Statement

Useful when selecting between multiple fixed options.

```csharp
int day = 2;

switch (day)
{
    case 1:
        Console.WriteLine("Monday");
        break;

    case 2:
        Console.WriteLine("Tuesday");
        break;

    case 3:
        Console.WriteLine("Wednesday");
        break;

    default:
        Console.WriteLine("Invalid Day");
        break;
}
```

`break` exits the switch after a matching case.
`default` executes when no case matches.

---

## 6. Switch Expression

Modern C# provides a more concise way to return a value.

```csharp
int day = 2;

string result = day switch
{
    1 => "Monday",
    2 => "Tuesday",
    3 => "Wednesday",
    _ => "Invalid Day"
};
```

`_` represents the **discard/default pattern**.

### Switch Statement vs Switch Expression

| Switch Statement      | Switch Expression        |
| --------------------- | ------------------------ |
| Executes statements   | Produces a value         |
| Uses `case` / `break` | Uses `=>`                |
| More verbose          | More concise             |
| Good for actions      | Good for value selection |

---

# 🧠 Tricky Interview Questions

### 1. `if-else` vs `switch` — when should you use each?

Use `if-else` for **ranges or complex conditions**.

Use `switch` for **multiple known options** based on a value or pattern.

### 2. Can `switch` work with strings?

Yes.

```csharp
string role = "Admin";

switch (role)
{
    case "Admin":
        Console.WriteLine("Full Access");
        break;
}
```

### 3. What happens if no case matches?

The `default` block executes, if one is provided.

### 4. Why is `break` used in a traditional switch?

It exits the current switch after a matching case.

### 5. Switch statement vs switch expression?

A **switch statement** executes code blocks, while a **switch expression** evaluates to a value.

### 6. Can multiple cases execute in C#?

Not through automatic fall-through like some languages. A traditional case normally ends with `break`, `return`, `goto`, or another valid exit.

### 7. What does `_` mean in a switch expression?

It is the **discard/default pattern** that matches when no previous pattern matches.

### 8. Can conditions be used with switch?

Yes. Modern C# supports **pattern matching** and `when` guards.

```csharp
string GetCategory(int age) => age switch
{
    < 13 => "Child",
    < 18 => "Teenager",
    _ => "Adult"
};
```

### 9. What is pattern matching?

Pattern matching allows `switch` to match values based on **type, value, relational conditions, and other patterns**.

### 10. Nested `if` vs `switch` — which is better?

Neither is always better.

Use `switch` for multiple discrete choices and `if` for ranges or complex boolean conditions.

---

## 🎯 Key Takeaways

```text
if          → Single condition
if-else     → Two possible paths
else if     → Multiple conditions
Nested if   → Condition inside another condition
switch      → Multiple fixed choices
switch expr → Concise value-based decision
Pattern     → Powerful modern switch matching
```

**Remember:** Choose the control structure that makes the logic easiest to understand and maintain.

## ✅ Checklist

* [x] `if`
* [x] `if-else`
* [x] `else if`
* [x] Nested conditions
* [x] `switch` statement
* [x] `switch` expression
* [x] Pattern matching
* [x] `default` and `_`
* [x] Tricky interview questions
