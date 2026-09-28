# Day 30 — Final Revision & Mock Interview 🎯

## 📌 Overview

Welcome to the final day of the **30 Days of C#** journey!

The goal of Day 30 is not to learn another new concept.

It is to **revise, connect and apply** everything learned throughout the series.

Today's focus:

* Revise important C# concepts
* Solve tricky interview questions
* Explain code before running it
* Practice coding problems
* Review common mistakes
* Discuss real-world .NET scenarios
* Practice thinking like a developer during an interview

---

# 🗺️ 30-Day C# Revision Roadmap

Throughout this series, the focus moved from C# fundamentals to practical .NET development.

```text
C# Fundamentals
      ↓
OOP
      ↓
Collections & Generics
      ↓
Exception Handling
      ↓
Delegates & Events
      ↓
LINQ
      ↓
Async / Await
      ↓
Advanced C#
      ↓
SOLID
      ↓
Dependency Injection
      ↓
Design Patterns
      ↓
Reflection / Attributes / Records
      ↓
Coding Challenges
      ↓
Real-World .NET Scenarios
      ↓
Final Revision & Mock Interview
```

---

# 1. C# Fundamentals — Quick Revision

### Important concepts

* Variables
* Data types
* Type conversion
* Operators
* Conditional statements
* Loops
* Methods
* Parameters
* Optional parameters
* Method overloading
* Access modifiers
* `var`
* `ref`
* `out`
* `in`

### Interview Question

**Q: What is the difference between `var` and `dynamic`?**

`var` is resolved at compile time.

`dynamic` is resolved at runtime.

```csharp
var name = "Ashish";
// Compiler knows this is a string.

dynamic value = "Ashish";
// Type checking happens at runtime.
```

---

# 2. OOP Revision

The four major pillars:

```text
Encapsulation
Inheritance
Polymorphism
Abstraction
```

### Encapsulation

Protecting an object's internal state and controlling how it is accessed.

### Inheritance

Creating a new class based on an existing class.

### Polymorphism

The same interface or method call can behave differently depending on the object.

### Abstraction

Showing essential behavior while hiding implementation details.

---

# 3. Interface vs Abstract Class

| Interface                              | Abstract Class                        |
| -------------------------------------- | ------------------------------------- |
| Defines a contract                     | Can provide contract + implementation |
| Multiple interfaces can be implemented | A class can inherit one base class    |
| Useful for loose coupling              | Useful for shared base behavior       |
| Commonly used with DI                  | Useful for common functionality       |

Example:

```csharp
public interface IPaymentService
{
    void Pay();
}
```

```csharp
public abstract class PaymentService
{
    public abstract void Pay();

    public void LogPayment()
    {
        Console.WriteLine("Payment logged.");
    }
}
```

---

# 4. Value Type vs Reference Type

### Value types

Examples:

```text
int
double
bool
struct
enum
```

### Reference types

Examples:

```text
class
string
array
object
delegate
```

The key difference is how values are stored and copied.

---

# 5. Collections Revision

Important collections:

| Collection                | Common Use            |
| ------------------------- | --------------------- |
| `List<T>`                 | Ordered collection    |
| `Dictionary<TKey,TValue>` | Key-value lookup      |
| `HashSet<T>`              | Unique values         |
| `Queue<T>`                | FIFO                  |
| `Stack<T>`                | LIFO                  |
| Array                     | Fixed-size collection |

Example:

```csharp
var employees = new List<string>
{
    "Ashish",
    "Rahul",
    "Priya"
};
```

---

# 6. Generics

Generics allow us to write reusable, type-safe code.

```csharp
public T GetValue<T>(T value)
{
    return value;
}
```

Usage:

```csharp
int number = GetValue(10);

string name = GetValue("Ashish");
```

### Why Generics?

* Type safety
* Reusability
* Less casting
* Better performance in many scenarios

---

# 7. Exception Handling

Important keywords:

```text
try
catch
finally
throw
```

Example:

```csharp
try
{
    int result = 10 / 0;
}
catch (DivideByZeroException)
{
    Console.WriteLine("Cannot divide by zero.");
}
```

### Interview Question

**Q: What is the difference between `throw;` and `throw ex;`?**

```csharp
throw;
```

Preserves the original stack trace.

```csharp
throw ex;
```

Can reset the stack trace from the rethrow location.

---

# 8. Delegates & Events

A delegate is a type-safe reference to a method.

```csharp
public delegate void Notify(string message);
```

Events are commonly used for notification-based communication.

```text
Publisher
    ↓
Event
    ↓
Subscriber
```

Common examples:

* UI events
* Notifications
* Messaging
* Callbacks

---

# 9. LINQ

LINQ allows us to query collections and data sources.

```csharp
var result = employees
    .Where(e => e.Salary > 50000)
    .OrderByDescending(e => e.Salary)
    .ToList();
```

Important methods:

```text
Where
Select
OrderBy
ThenBy
GroupBy
Any
All
First
FirstOrDefault
Single
Count
Sum
Max
Min
ToList
```

### Interview Question

**Q: What is deferred execution?**

Some LINQ queries are not executed when they are created.

They execute when the result is enumerated.

```csharp
var result = employees
    .Where(e => e.Salary > 50000);

// Query executes here
var list = result.ToList();
```

---

# 10. Async / Await

Asynchronous programming is important for I/O operations.

```csharp
public async Task<Employee> GetEmployeeAsync(int id)
{
    return await repository.GetByIdAsync(id);
}
```

Important concepts:

* `Task`
* `Task<T>`
* `async`
* `await`
* `Task.WhenAll`
* Cancellation
* Exception handling

### Avoid unnecessarily blocking async code

```csharp
.Result
.Wait()
```

Prefer:

```csharp
await
```

---

# 11. SOLID Principles

```text
S → Single Responsibility Principle
O → Open/Closed Principle
L → Liskov Substitution Principle
I → Interface Segregation Principle
D → Dependency Inversion Principle
```

The goal is to create code that is easier to:

* Maintain
* Extend
* Test
* Understand

---

# 12. Dependency Injection

Instead of creating dependencies manually:

```csharp
var repository = new EmployeeRepository();
```

Inject them:

```csharp
public EmployeeService(
    IEmployeeRepository repository)
{
    _repository = repository;
}
```

ASP.NET Core provides a built-in DI container.

Common lifetimes:

```text
Transient
Scoped
Singleton
```

---

# 13. Design Patterns

Important patterns covered:

### Singleton

One shared instance.

### Factory

Creates objects without exposing the creation logic.

### Adapter

Allows incompatible interfaces to work together.

### Observer

Allows objects to subscribe to notifications.

Patterns are reusable solutions to recurring software design problems.

---

# 14. Reflection

Reflection allows a program to inspect types at runtime.

```csharp
Type type = typeof(Employee);

var properties = type.GetProperties();

foreach (var property in properties)
{
    Console.WriteLine(property.Name);
}
```

Common uses:

* Frameworks
* Dependency Injection
* Serialization
* Testing
* Attribute processing

---

# 15. Attributes

Attributes add metadata to code.

Example:

```csharp
[Obsolete("Use the new method instead.")]
public void OldMethod()
{
}
```

Custom attributes can also be created.

```csharp
public class AuthorAttribute : Attribute
{
    public string Name { get; }

    public AuthorAttribute(string name)
    {
        Name = name;
    }
}
```

---

# 16. Records

Records are useful for data-focused models and value-based equality.

```csharp
public record Employee(
    int Id,
    string Name);
```

The `with` expression creates a modified copy:

```csharp
var updatedEmployee = employee with
{
    Name = "Rahul"
};
```

---

# 17. Debugging Revision

Important Visual Studio tools:

```text
Breakpoint
Step Into
Step Over
Step Out
Watch
Quick Watch
Locals
Call Stack
Immediate Window
Exception Settings
Diagnostic Tools
```

### Shortcuts

| Shortcut    | Action            |
| ----------- | ----------------- |
| F5          | Continue          |
| F9          | Toggle breakpoint |
| F10         | Step Over         |
| F11         | Step Into         |
| Shift + F11 | Step Out          |

---

# 🧠 Mock Interview — Tricky Questions

## Question 1

What is the output?

```csharp
int x = 10;

Console.WriteLine(x++);

Console.WriteLine(++x);
```

### Answer

```text
10
12
```

Explanation:

```text
x++ → use 10, then x becomes 11

++x → x becomes 12, then use 12
```

---

# Question 2

What is the output?

```csharp
string a = "Hello";
string b = "Hello";

Console.WriteLine(a == b);
```

### Answer

```text
True
```

For strings, `==` compares string values.

---

# Question 3

What happens here?

```csharp
var numbers = new List<int>
{
    1, 2, 3
};

var result = numbers
    .Where(x => x > 1);

numbers.Add(4);

Console.WriteLine(
    string.Join(", ", result));
```

### Answer

```text
2, 3, 4
```

Why?

Because LINQ `Where()` uses deferred execution.

The query is evaluated when it is enumerated.

---

# Question 4

What is wrong with this code?

```csharp
try
{
    ProcessData();
}
catch
{
}
```

### Answer

The exception is silently swallowed.

This makes:

* Debugging difficult
* Errors invisible
* Production issues harder to investigate

A better approach is to handle the exception meaningfully or log it.

---

# Question 5

What is the output?

```csharp
var numbers = new List<int>
{
    1, 2, 3, 4, 5
};

var result = numbers
    .Where(x => x % 2 == 0)
    .Select(x => x * 10);

Console.WriteLine(
    string.Join(", ", result));
```

### Answer

```text
20, 40
```

---

# Question 6

What is wrong with this?

```csharp
public async Task GetData()
{
    var result = GetDataFromDatabase().Result;
}
```

### Answer

The code is unnecessarily blocking an asynchronous operation.

Prefer:

```csharp
public async Task GetData()
{
    var result = await GetDataFromDatabase();
}
```

---

# Question 7

Which principle is violated if one class handles:

```text
Employee CRUD
Email Sending
PDF Generation
Authentication
Logging
```

### Answer

Primarily **Single Responsibility Principle**.

The class has too many unrelated responsibilities.

---

# Question 8

What is the difference between `IEnumerable<T>` and `IQueryable<T>`?

### `IEnumerable<T>`

Generally works with data already in memory.

### `IQueryable<T>`

Can build an expression tree that a provider such as EF Core can translate to a database query.

Example:

```csharp
IQueryable<Employee> query =
    context.Employees;

var result = await query
    .Where(e => e.Salary > 50000)
    .ToListAsync();
```

---

# Question 9

What is the difference between `FirstOrDefault()` and `SingleOrDefault()`?

### `FirstOrDefault()`

Returns the first matching element or default value.

It does not require uniqueness.

### `SingleOrDefault()`

Returns the only matching element or default value.

It throws an exception if multiple matching elements exist.

---

# Question 10

When should you use `Dictionary<TKey,TValue>` instead of `List<T>`?

Use a dictionary when you need efficient lookup based on a key.

Example:

```csharp
var employees = new Dictionary<int, string>
{
    { 1, "Ashish" },
    { 2, "Rahul" }
};

Console.WriteLine(employees[1]);
```

---

# 💻 Mock Coding Round

## Problem 1 — Find the First Non-Repeating Character

### Question

Given:

```text
"swiss"
```

Return:

```text
"w"
```

### Solution

```csharp
public static char? FirstNonRepeatingCharacter(
    string input)
{
    var counts = input
        .GroupBy(c => c)
        .ToDictionary(
            g => g.Key,
            g => g.Count());

    foreach (char character in input)
    {
        if (counts[character] == 1)
            return character;
    }

    return null;
}
```

---

# Problem 2 — Remove Duplicates

### Question

Input:

```text
[1, 2, 2, 3, 4, 4, 5]
```

Output:

```text
[1, 2, 3, 4, 5]
```

### Solution

```csharp
var result = numbers
    .Distinct()
    .ToList();
```

---

# Problem 3 — Find the Second Largest Number

### Solution

```csharp
public static int SecondLargest(
    List<int> numbers)
{
    return numbers
        .Distinct()
        .OrderByDescending(x => x)
        .Skip(1)
        .First();
}
```

For production code, validate that there are at least two distinct values before calling `First()`.

---

# 🌐 Real-World Scenario 1 — Slow API

### Problem

An employee API takes 5 seconds to respond.

### How would you investigate?

```text
Check Logs
    ↓
Measure API
    ↓
Inspect EF Core Query
    ↓
Check SQL Query
    ↓
Check Indexes
    ↓
Check Data Volume
    ↓
Check External Services
    ↓
Consider Caching
    ↓
Measure Again
```

Don't optimize blindly.

First identify the bottleneck.

---

# 🌐 Real-World Scenario 2 — API Returns 500

### Problem

An API suddenly starts returning HTTP 500.

### Investigation

```text
Check Application Logs
        ↓
Find Exception
        ↓
Check Stack Trace
        ↓
Reproduce Locally
        ↓
Debug
        ↓
Fix Root Cause
        ↓
Test
        ↓
Deploy
```

The important principle is:

> Don't just hide the exception. Find the root cause.

---

# 🌐 Real-World Scenario 3 — Database Called Too Many Times

### Problem

An API loads 100 employees and makes many additional database calls.

Possible cause:

```text
N + 1 Query Problem
```

Possible improvements include:

* Proper projection
* Appropriate eager loading
* Query restructuring
* Reviewing generated SQL
* Loading only required data

Always inspect the actual query and data requirements before choosing the solution.

---

# 🌐 Real-World Scenario 4 — Authentication

### Requirement

Only Admin users should access:

```text
/api/admin/reports
```

Possible approach:

```csharp
[Authorize(Roles = "Admin")]
[HttpGet("reports")]
public IActionResult GetReports()
{
    return Ok();
}
```

Flow:

```text
Login
 ↓
Authentication
 ↓
JWT / Cookie
 ↓
Authorization
 ↓
Role Check
 ↓
Allow / Deny
```

---

# 🎯 Final Interview Checklist

Before an interview, make sure you can explain:

### C# Fundamentals

* [ ] Value vs reference types
* [ ] `var` vs `dynamic`
* [ ] `ref`, `out`, `in`
* [ ] Access modifiers
* [ ] Method overloading

### OOP

* [ ] Encapsulation
* [ ] Inheritance
* [ ] Polymorphism
* [ ] Abstraction
* [ ] Interface vs abstract class

### Collections & Generics

* [ ] List
* [ ] Dictionary
* [ ] HashSet
* [ ] Queue
* [ ] Stack
* [ ] Generics
* [ ] Constraints

### Advanced C#

* [ ] Delegates
* [ ] Events
* [ ] LINQ
* [ ] Async/Await
* [ ] Exception Handling
* [ ] Reflection
* [ ] Attributes
* [ ] Records

### Architecture & Design

* [ ] SOLID
* [ ] Dependency Injection
* [ ] Design Patterns
* [ ] Repository Pattern
* [ ] Separation of Concerns

### .NET

* [ ] ASP.NET Core Web API
* [ ] Middleware
* [ ] Authentication
* [ ] Authorization
* [ ] EF Core
* [ ] SQL Server
* [ ] Logging
* [ ] Caching
* [ ] Background Services

### Interview Skills

* [ ] Explain your approach before coding
* [ ] Handle edge cases
* [ ] Explain time and space complexity
* [ ] Debug your own code
* [ ] Explain your projects clearly
* [ ] Connect concepts to real-world scenarios

---

# 🏆 30-Day Journey — Completed!

```text
Day 01 → C# Fundamentals
Day 02 → ...
   ↓
Day 20 → Delegates & Events
Day 21 → LINQ
Day 22 → Async / Await
Day 23 → Collections & Advanced Generics
Day 24 → SOLID Principles
Day 25 → Dependency Injection & Design Patterns
Day 26 → Reflection, Attributes & Records
Day 27 → Advanced Exception Handling & Debugging
Day 28 → Practical Coding Challenges
Day 29 → Real-World C# & .NET Scenarios
Day 30 → Final Revision & Mock Interview
```

---

# 💡 Final Takeaway

The biggest lesson from these 30 days is:

> **Don't just learn C#. Learn how to think like a C# developer.**

A strong developer should be able to:

```text
Understand the Problem
        ↓
Design the Solution
        ↓
Write Clean Code
        ↓
Handle Edge Cases
        ↓
Debug Problems
        ↓
Optimize When Needed
        ↓
Explain the Solution
```

Knowing syntax is only the beginning.

The real skill is being able to **apply the right concept to the right problem**.

---

# 🎉 30/30 COMPLETED!

**30 Days. 30 Topics. One stronger C# foundation.**

From fundamentals to advanced C#, coding challenges and real-world .NET scenarios — this journey helped build a stronger foundation for **professional .NET development and technical interviews**.

The 30-day challenge ends here.

**The learning doesn't. 🚀**


