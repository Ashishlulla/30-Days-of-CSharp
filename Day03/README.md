# 🚀 30 Days of C# — Day 3/30

📅 **Date:** 3 September 2026
📚 **Topic:** Operators & Expressions

## 📌 Overview

Operators are symbols used to perform operations on values and variables. Understanding operator behavior, precedence, and evaluation order is important for writing correct and readable C# code.

---

## 1. Arithmetic Operators

Used for mathematical operations:

```csharp
int a = 10;
int b = 3;

a + b;  // 13
a - b;  // 7
a * b;  // 30
a / b;  // 3
a % b;  // 1
```

⚠️ When both operands are integers, `/` performs **integer division**.

```csharp
10 / 3    // 3
10.0 / 3  // 3.333...
```

---

## 2. Comparison Operators

Used to compare values:

```text
==   !=   >   <   >=   <=
```

Example:

```csharp
int a = 10;
int b = 20;

a < b;   // true
a == b;  // false
```

---

## 3. Logical Operators

```text
&&   Logical AND
||   Logical OR
!    Logical NOT
```

Example:

```csharp
int age = 25;

bool result = age >= 18 && age <= 60;
```

### Short-Circuiting

With `&&` and `||`, C# may skip evaluating the second condition when the result is already known.

```csharp
if (user != null && user.IsActive)
{
    // ...
}
```

---

## 4. Assignment Operators

```text
=   +=   -=   *=   /=   %=
```

Example:

```csharp
int x = 10;

x += 5;   // 15
x -= 3;   // 12
x *= 2;   // 24
x /= 4;   // 6
```

---

## 5. Increment & Decrement

```csharp
int i = 5;

++i;  // Pre-increment
i++;  // Post-increment

--i;  // Pre-decrement
i--;  // Post-decrement
```

**Pre:** change first, then use.
**Post:** use first, then change.

---

## 6. Ternary Operator

Short form of a simple `if-else`:

```csharp
int age = 20;

string result = age >= 18 ? "Adult" : "Minor";
```

Syntax:

```text
condition ? trueExpression : falseExpression
```

---

## 7. Null Operators

### Null Coalescing `??`

Returns the right side when the left side is `null`.

```csharp
string name = null;

string result = name ?? "Guest";
// Guest
```

### Null Coalescing Assignment `??=`

Assigns a value only when the variable is `null`.

```csharp
string name = null;

name ??= "Guest";
// name = "Guest"
```

### Null Conditional `?.`

Safely accesses a member when an object may be `null`.

```csharp
int? length = user?.Name?.Length;
```

---

## 8. Operator Precedence

When multiple operators appear in an expression, precedence determines which operators are evaluated first.

Example:

```csharp
int result = 2 + 3 * 4;
// 14
```

`*` is evaluated before `+`.

### Best Practice

Use parentheses when the intended order is important:

```csharp
int result = (2 + 3) * 4;
// 20
```

---

# 🧠 Tricky Interview Questions

### 1. `==` vs `.Equals()`?

`==` uses the equality operator and may be overloaded. `.Equals()` checks equality according to the type's implementation.

### 2. `&&` vs `&`?

`&&` is conditional AND and supports short-circuiting.
`&` can perform logical AND on `bool` values but evaluates both operands; with integral types, it performs bitwise AND.

### 3. What is short-circuit evaluation?

When C# can determine the result without evaluating the remaining conditions.

### 4. What is `10 / 3`?

```text
3
```

Because both operands are integers.

### 5. `++i` vs `i++`?

```text
++i → Increment, then use
i++ → Use, then increment
```

### 6. What does `??` do?

Returns the left operand if it isn't `null`; otherwise returns the right operand.

### 7. `??` vs `?:`?

`??` is mainly for **null handling**.
`?:` evaluates a **condition** and selects between two expressions.

### 8. What does `?.` do?

Provides null-conditional access and helps avoid `NullReferenceException` when accessing members of a potentially null object.

### 9. What is operator precedence?

The rules that determine the order in which operators are evaluated.

### 10. Should you write complex expressions like this?

```csharp
int result = x++ + ++x;
```

Avoid such expressions in production code. Even when the language defines the evaluation, they are difficult to read and maintain.

---

## 🎯 Key Takeaways

```text
Arithmetic     → + - * / %
Comparison     → == != > < >= <=
Logical        → && || !
Assignment     → = += -= *= /= %=
Increment      → ++ --
Ternary        → ?:
Null handling  → ?? ??= ?.
```

**Remember:** Don't just memorize operators. Understand **type behavior, short-circuiting, precedence, and evaluation order**.

## ✅ Checklist

* [x] Arithmetic operators
* [x] Comparison operators
* [x] Logical operators
* [x] Short-circuiting
* [x] Assignment operators
* [x] Increment/decrement
* [x] Ternary operator
* [x] Null operators
* [x] Operator precedence
* [x] Tricky interview questions
