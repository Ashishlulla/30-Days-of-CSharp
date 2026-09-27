# Day 27 — Exception Handling & Debugging — Advanced

## 📌 Overview

Exception handling and debugging are essential skills for building reliable C# applications.

Exception handling helps us deal with unexpected runtime problems gracefully, while debugging helps us find and fix the root cause of issues.

On Day 27, I explored advanced exception handling techniques and important debugging features available in Visual Studio.

---

# 1. What is Exception Handling?

An **exception** is an unexpected problem that occurs while a program is running.

Exception handling allows us to:

* Detect runtime errors
* Handle errors gracefully
* Prevent application crashes
* Log useful error information
* Recover from errors when possible

### Basic flow

```text
Exception occurs
       ↓
   Catch it
       ↓
 Process / Log
       ↓
 Recover or Fail Gracefully
```

---

# 2. try-catch-finally

The `try` block contains code that may throw an exception.

The `catch` block handles the exception.

The `finally` block executes whether an exception occurs or not.

```csharp
try
{
    int result = 10 / 0;
}
catch (DivideByZeroException ex)
{
    Console.WriteLine("Cannot divide by zero.");
}
finally
{
    Console.WriteLine("This always runs.");
}
```

### Structure

```csharp
try
{
    // Code that may throw an exception
}
catch (Exception ex)
{
    // Handle exception
}
finally
{
    // Cleanup code
}
```

---

# 3. Common Exception Types

| Exception                   | When it occurs                              |
| --------------------------- | ------------------------------------------- |
| `NullReferenceException`    | Accessing a member of a null object         |
| `DivideByZeroException`     | Division by zero                            |
| `FormatException`           | Invalid data format                         |
| `ArgumentException`         | Invalid argument passed to a method         |
| `ArgumentNullException`     | Null argument passed where it isn't allowed |
| `InvalidOperationException` | Operation is invalid for the current state  |
| `FileNotFoundException`     | Requested file does not exist               |

### Example

```csharp
int number = int.Parse("abc");
```

This can throw:

```text
FormatException
```

---

# 4. Throwing Exceptions

We can use the `throw` keyword when a condition is invalid.

```csharp
public void CheckAge(int age)
{
    if (age < 18)
    {
        throw new ArgumentException(
            "Age must be 18 or above.");
    }
}
```

### Why use `throw`?

It allows a method to communicate that something went wrong and lets the calling code decide how to handle it.

---

# 5. Custom Exceptions

We can create our own exception classes for specific application or business rules.

```csharp
public class InvalidAgeException : Exception
{
    public InvalidAgeException(string message)
        : base(message)
    {
    }
}
```

Usage:

```csharp
if (age < 18)
{
    throw new InvalidAgeException(
        "Age must be 18 or above.");
}
```

### When to use custom exceptions?

Custom exceptions can be useful when:

* A business rule needs a specific error type
* The same error needs to be identified in multiple places
* The application needs specialized exception handling

---

# 6. Exception Filters

Exception filters allow us to handle an exception only when a specific condition is true.

```csharp
try
{
    // Code
}
catch (Exception ex) when (ex.Message.Contains("database"))
{
    Console.WriteLine("Database error.");
}
```

The `when` keyword adds a condition to the `catch` block.

---

# 7. Exception Handling Best Practices

### 1. Catch specific exceptions

Prefer:

```csharp
catch (FileNotFoundException ex)
{
}
```

Instead of unnecessarily catching everything:

```csharp
catch (Exception ex)
{
}
```

---

### 2. Don't swallow exceptions

Avoid:

```csharp
try
{
    // Code
}
catch
{
}
```

An empty catch block hides the actual problem.

---

### 3. Preserve the original exception

When rethrowing an exception:

```csharp
catch (Exception ex)
{
    Log(ex);
    throw;
}
```

Prefer `throw;` when you want to preserve the original stack trace.

---

### 4. Use finally for cleanup

```csharp
try
{
    // Use resource
}
finally
{
    // Cleanup
}
```

For many disposable resources, prefer `using` / `using` declarations.

```csharp
using var connection = new SqlConnection(connectionString);
```

---

### 5. Keep try blocks focused

A large `try` block can make it difficult to understand which operation caused the exception.

Keep exception handling close to the operation that actually needs it.

---

### 6. Log useful information

A production application should record useful details such as:

* Exception type
* Message
* Stack trace
* Request/context information
* Relevant identifiers

Avoid exposing sensitive information to end users.

---

# 8. Debugging in Visual Studio

Debugging helps us understand what the program is actually doing at runtime.

## Breakpoints

A breakpoint pauses execution at a specific line.

You can:

* Click the left margin
* Press `F9`

Example:

```csharp
int result = CalculateSalary();
```

Place a breakpoint on this line to inspect the application state before execution continues.

---

# 9. Step Through Code

Visual Studio provides several useful debugging commands.

| Command           | Shortcut      | Purpose                  |
| ----------------- | ------------- | ------------------------ |
| Step Over         | `F10`         | Execute the current line |
| Step Into         | `F11`         | Enter a method           |
| Step Out          | `Shift + F11` | Exit the current method  |
| Continue          | `F5`          | Continue execution       |
| Toggle Breakpoint | `F9`          | Add/remove breakpoint    |

### Example

```csharp
CalculateSalary();
SendEmail();
SaveEmployee();
```

Using **Step Into** allows you to enter `CalculateSalary()` and inspect its internal execution.

---

# 10. Watch & Quick Watch

## Watch Window

The Watch window allows you to monitor variables and expressions while debugging.

Example:

```csharp
int age = 25;
string name = "Ashish";
```

You can watch:

```text
age
name
```

---

## Quick Watch

Quick Watch allows you to quickly inspect the value of an expression while debugging.

For example:

```csharp
employee.Name
```

This is useful when troubleshooting complex objects or expressions.

---

# 11. Call Stack

The **Call Stack** shows the sequence of method calls that led to the current point.

Example:

```text
Main()
  ↓
ProcessEmployee()
  ↓
CalculateSalary()
  ↓
ValidateSalary()
```

If an exception occurs inside `ValidateSalary()`, the Call Stack helps identify how the application reached that method.

---

# 12. Conditional Breakpoints

Sometimes we don't want a breakpoint to stop every time.

A conditional breakpoint can pause execution only when a condition is true.

Example:

```csharp
employee.Id == 100
```

This is useful when debugging loops or applications processing many records.

---

# 13. Exception Settings

Visual Studio can be configured to break when exceptions are thrown.

This helps identify the exact location where an exception occurs instead of waiting until it reaches another handler.

Useful when investigating:

* Unexpected exceptions
* Unhandled exceptions
* Exceptions hidden by higher-level error handling

---

# 14. Important Visual Studio Debugging Windows

| Window             | Purpose                            |
| ------------------ | ---------------------------------- |
| Locals             | View local variables               |
| Watch              | Monitor selected variables         |
| Immediate          | Evaluate expressions               |
| Call Stack         | View method call sequence          |
| Output             | View application/debug output      |
| Exception Settings | Configure exception break behavior |
| Diagnostic Tools   | Analyze performance and memory     |

---

# 15. Exception Handling vs Debugging

| Exception Handling                | Debugging                                      |
| --------------------------------- | ---------------------------------------------- |
| Handles runtime problems          | Finds the cause of problems                    |
| Uses `try-catch-finally`          | Uses breakpoints and debugging tools           |
| Helps application fail gracefully | Helps developers understand execution          |
| Important in production           | Mainly used during development/troubleshooting |
| Can log errors                    | Helps inspect variables and call flow          |

Both are important for reliable software development.

---

# 16. Practical Example

```csharp
public decimal CalculateSalary(decimal salary, decimal bonus)
{
    if (salary < 0)
    {
        throw new ArgumentException(
            "Salary cannot be negative.");
    }

    return salary + bonus;
}
```

Calling code:

```csharp
try
{
    decimal total = CalculateSalary(-5000, 1000);

    Console.WriteLine(total);
}
catch (ArgumentException ex)
{
    Console.WriteLine(ex.Message);
}
```

Here:

1. `CalculateSalary()` validates the input.
2. It throws an `ArgumentException`.
3. The caller catches the exception.
4. The error message is handled gracefully.

---

# 17. Interview Questions

### Q1. What is the difference between `throw;` and `throw ex;`?

`throw;` preserves the original stack trace.

`throw ex;` resets the stack trace from the point where it is rethrown.

---

### Q2. Why should we avoid an empty catch block?

Because it silently hides errors and makes debugging much harder.

---

### Q3. What is an exception filter?

An exception filter uses the `when` keyword to handle an exception only when a specific condition is satisfied.

---

### Q4. When should you create a custom exception?

When the application has a meaningful, specialized error condition that benefits from its own exception type.

---

### Q5. What is the difference between Step Into and Step Over?

**Step Into** enters the called method.

**Step Over** executes the method without entering its internal code.

---

### Q6. What is the purpose of the Call Stack?

It shows the chain of method calls that led to the current execution point.

---

### Q7. What is a conditional breakpoint?

A breakpoint that pauses execution only when a specified condition is true.

---

### Q8. Should every exception be caught?

No. Exceptions should generally be handled where the application can meaningfully recover, log, translate, or respond to them.

---

# 💡 Key Takeaway

**Exception handling** helps applications deal with runtime failures gracefully.

**Debugging** helps developers understand what happened and find the root cause.

Together, they help us build C# applications that are:

* Reliable
* Maintainable
* Easier to troubleshoot
* More production-ready

> **Handle exceptions wisely. Debug systematically. Build reliably.**

---

