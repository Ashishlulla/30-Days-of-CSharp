# 📘 Day 19 — Exception Handling in C#

**Exception Handling** is a mechanism used to handle runtime errors and prevent unexpected application crashes.

It helps us maintain the normal flow of an application and provide meaningful error messages.

## 🛡️ Why Do We Use Exception Handling?

Exception handling helps us:

* Handle runtime errors gracefully.
* Prevent unexpected application termination.
* Provide meaningful error messages.
* Separate error-handling code from business logic.
* Clean up resources when an error occurs.

## 1. What Is an Exception?

An exception is an object that represents an error occurring during program execution.

Examples:

* Division by zero.
* Accessing a null object.
* File not found.
* Invalid input.
* Index outside an array's bounds.

All exceptions derive from the `System.Exception` class.

## 2. try-catch-finally

C# provides three main blocks for exception handling.

| Block     | Purpose                                   |
| --------- | ----------------------------------------- |
| `try`     | Contains code that may throw an exception |
| `catch`   | Handles an exception                      |
| `finally` | Executes cleanup code                     |

### Example

```csharp
try
{
    int result = 10 / 0;
    Console.WriteLine(result);
}
catch (DivideByZeroException ex)
{
    Console.WriteLine("Cannot divide by zero.");
}
finally
{
    Console.WriteLine("Execution completed.");
}
```

**Output:**

```text
Cannot divide by zero.
Execution completed.
```

The `finally` block executes whether an exception occurs or not, except in unusual termination scenarios.

## 3. Multiple Catch Blocks

We can use multiple catch blocks to handle different exception types.

```csharp
try
{
    int[] numbers = { 10, 20, 30 };

    Console.WriteLine(numbers[5]);
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine("Index is outside the array.");
}
catch (Exception ex)
{
    Console.WriteLine("Something went wrong.");
}
```

### Important

Specific exception types should generally be caught before a general `Exception` catch.

## 4. The `throw` Keyword

The `throw` keyword is used to create or rethrow an exception.

```csharp
public void CheckAge(int age)
{
    if (age < 18)
    {
        throw new ArgumentException("Age must be 18 or above.");
    }
}
```

Calling the method with an invalid age throws an exception.

## 5. Rethrowing Exceptions

Inside a catch block, `throw;` rethrows the current exception while preserving its original stack trace.

```csharp
try
{
    // Risky operation
}
catch (Exception)
{
    // Log the exception

    throw;
}
```

### `throw` vs `throw ex`

| `throw;`                           | `throw ex;`                                           |
| ---------------------------------- | ----------------------------------------------------- |
| Rethrows the current exception     | Throws the exception object again                     |
| Preserves the original stack trace | Resets the stack trace from the rethrow point         |
| Preferred when rethrowing          | Generally avoid when preserving debugging information |

## 6. Custom Exceptions

We can create custom exceptions by inheriting from `Exception`.

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
public void SetAge(int age)
{
    if (age < 18)
    {
        throw new InvalidAgeException(
            "Age must be 18 or above.");
    }
}
```

Custom exceptions are useful when an application needs to represent a specific business or domain error.

## 7. Common Exception Classes

| Exception                   | When It Occurs                             |
| --------------------------- | ------------------------------------------ |
| `DivideByZeroException`     | Division by zero                           |
| `NullReferenceException`    | Accessing members through a null reference |
| `IndexOutOfRangeException`  | Invalid array index                        |
| `ArgumentException`         | Invalid argument                           |
| `InvalidOperationException` | Invalid operation for the current state    |
| `FileNotFoundException`     | Requested file cannot be found             |

## 8. Exception Handling Best Practices

* Catch specific exception types when possible.
* Keep `try` blocks small and focused.
* Avoid catching `Exception` without a clear reason.
* Use `finally` or `using` for resource cleanup.
* Log useful exception details.
* Do not silently ignore exceptions.
* Avoid using exceptions for normal program flow.
* Do not expose sensitive information in error messages.

## 9. Real-World Example — File Handling

```csharp
try
{
    string content = File.ReadAllText("data.txt");

    Console.WriteLine(content);
}
catch (FileNotFoundException)
{
    Console.WriteLine("The file was not found.");
}
catch (IOException)
{
    Console.WriteLine("An I/O error occurred.");
}
```

The application handles expected file-related errors without crashing unexpectedly.

## 🧠 Interview Questions

### Q1. What is exception handling in C#?

It is a mechanism for handling runtime errors and maintaining the normal flow of an application.

### Q2. What is the purpose of `finally`?

The `finally` block is used for cleanup operations and normally executes whether an exception occurs or not.

### Q3. What is the difference between `throw` and `throw ex`?

`throw;` preserves the original stack trace when rethrowing an exception. `throw ex;` resets the stack trace from the rethrow point.

### Q4. Can we have multiple catch blocks?

**Yes.** We can handle different exception types using multiple catch blocks.

### Q5. What is the base class of all exceptions in C#?

`System.Exception`.

### Q6. Why should we avoid empty catch blocks?

They silently ignore errors, making debugging and maintenance difficult.

## ⭐ Key Takeaway

> **Exception handling helps us handle errors gracefully, keep applications running safely, and provide meaningful feedback to users.**

---

