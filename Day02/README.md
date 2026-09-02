# 🚀 30 Days of C# — Day 2/30

📅 **Date:** 2 September 2026
📚 **Topic:** Variables, Data Types & Type Conversion

---

## 📌 Variables

Variables store values in memory.

```csharp
int age = 25;
string name = "Ashish";
```

**`const` vs `readonly`**

* `const` → Compile-time constant
* `readonly` → Can be assigned during declaration or in a constructor

---

## 🔵 Value Types vs Reference Types

### Value Types

A value type variable **contains its own value**.

When you assign it to another variable, the value is copied.

Examples:

`int`, `double`, `bool`, `char`, `struct`, `enum`

```csharp
int a = 10;
int b = a;

b = 20;

// a = 10
// b = 20
```

Changing `b` does not affect `a`.

### Reference Types

A reference type variable **holds a reference to an object**.

When you assign it to another variable, both variables can refer to the **same object**.

Examples:

`class`, `string`, `array`, `object`, `interface`

```csharp
Person p1 = new Person();
Person p2 = p1;

p2.Name = "John";

// p1.Name = "John"
// p2.Name = "John"
```

### 🧠 Easy Way to Remember

```text
Value Type
   ↓
Copies the value

Reference Type
   ↓
Copies the reference
```

> ⚠️ Don't simply remember "stack vs heap". The important concept is **value copying vs reference sharing**. Actual memory behavior is more nuanced.

---

## 🟠 Type Conversion

### Implicit Conversion

```csharp
int x = 10;
double y = x;
```

Safe conversion performed automatically.

### Explicit Conversion

```csharp
double x = 10.75;
int y = (int)x;  // 10
```

⚠️ May cause data loss.

---

## 🔄 Boxing & Unboxing

```csharp
int x = 10;

object obj = x;     // Boxing
int y = (int)obj;   // Unboxing
```

* **Boxing:** Value Type → Object
* **Unboxing:** Object → Value Type

---

## 🧠 `var` vs `dynamic` vs `object`

| Keyword   | Type Resolution |
| --------- | --------------- |
| `var`     | Compile time    |
| `dynamic` | Runtime         |
| `object`  | Base type       |

```csharp
var x = 10;          // int

dynamic value = 10;
value = "Hello";     // Allowed

object data = 10;
```

⚠️ `var` is **not** dynamically typed.

---

## 🟣 Nullable Types

```csharp
int? age = null;
int? score = 95;
```

`int?` is shorthand for `Nullable<int>`.

---

## ⚠️ Tricky Interview Questions

**1. Is `var` dynamically typed?**
❌ No. Its type is determined at compile time.

**2. Value Type vs Reference Type?**
Value types copy the value; reference types copy the reference.

**3. What is boxing?**
Converting a value type into an `object`.

**4. `var` vs `dynamic`?**
`var` → Compile time
`dynamic` → Runtime

**5. `const` vs `readonly`?**
`const` → Compile-time constant
`readonly` → Assigned once.

**6. Is `string` a value type?**
❌ No. `string` is a reference type.

**7. What is `int?`?**
`Nullable<int>` — allows an integer or `null`.

---

## 🎯 Key Takeaways

```text
Value Type     → Copies the value
Reference Type → Copies the reference

var      → Compile-time type inference
dynamic  → Runtime type resolution
object   → Base type

Boxing   → Value Type → Object
Unboxing → Object → Value Type

const    → Compile-time constant
readonly → Assigned once
```

> **Understand how C# stores, copies, and converts data — don't just memorize syntax.**
