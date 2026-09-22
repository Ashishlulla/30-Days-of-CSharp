# Day 22 — Async / Await in C#

## Overview

`async` and `await` are C# keywords used to write asynchronous code in a simple and readable way.

Asynchronous programming allows an application to start a long-running operation without blocking the current thread while waiting for the operation to finish.

It is especially useful for I/O-bound operations such as:

* Calling APIs
* Reading and writing files
* Accessing databases
* Sending emails
* Network communication

---

## Why Use Async / Await?

The main benefits are:

* Keeps applications responsive
* Avoids blocking threads during I/O operations
* Improves scalability in web applications
* Makes asynchronous code easier to read
* Supports efficient API and database calls
* Helps multiple independent operations run concurrently

---

## Important Concepts

| Concept          | Meaning                                                   |
| ---------------- | --------------------------------------------------------- |
| `Task`           | Represents an asynchronous operation                      |
| `Task<T>`        | Represents an asynchronous operation that returns a value |
| `async`          | Marks a method as asynchronous                            |
| `await`          | Waits asynchronously for a task to complete               |
| `Task.WhenAll()` | Waits for multiple tasks to complete                      |
| `Task.Delay()`   | Creates an asynchronous delay                             |
| `Task.Run()`     | Schedules work on a thread-pool thread                    |

---

## Basic Async Example

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main()
    {
        string result = await GetDataAsync();

        Console.WriteLine(result);
    }

    static async Task<string> GetDataAsync()
    {
        await Task.Delay(2000);

        return "Data loaded";
    }
}
```

### Output

```text
Data loaded
```

`Task.Delay()` pauses the asynchronous operation without blocking the thread.

---

## Async Method Return Types

An asynchronous method commonly returns:

### Task

Used when the method does not return a value.

```csharp
public async Task SaveDataAsync()
{
    await Task.Delay(1000);

    Console.WriteLine("Data saved");
}
```

### Task<T>

Used when the method returns a value.

```csharp
public async Task<int> GetNumberAsync()
{
    await Task.Delay(1000);

    return 10;
}
```

### ValueTask<T>

`ValueTask<T>` can be useful in performance-sensitive scenarios where a result may already be available. It should not be used automatically for every async method.

---

## Understanding await

The `await` keyword asynchronously waits for a task to complete.

```csharp
public async Task<string> GetMessageAsync()
{
    var message = await GetMessageFromServerAsync();

    return message;
}
```

The method returns control to its caller while the asynchronous operation is in progress.

When the operation completes, the method continues from the point where it awaited the task.

---

## Working with HttpClient

`HttpClient` provides asynchronous methods for making HTTP requests.

```csharp
using System.Net.Http;

public async Task<string> GetDataFromApiAsync()
{
    using var client = new HttpClient();

    string response = await client.GetStringAsync(
        "https://api.example.com/data"
    );

    return response;
}
```

For production applications, `HttpClient` should generally be reused or created through `IHttpClientFactory`.

---

## Async Database Operations

Entity Framework Core supports asynchronous database operations.

```csharp
public async Task<List<Doctor>> GetDoctorsAsync()
{
    return await _context.Doctors
        .Where(doctor => doctor.IsActive)
        .ToListAsync();
}
```

Required namespaces:

```csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;
using System.Threading.Tasks;
using System.Linq;
```

Async database methods help avoid blocking web-server threads while the database is processing a query.

---

## Running Multiple Async Operations

When operations are independent, they can be started before awaiting them.

```csharp
public async Task LoadDataAsync()
{
    Task<string> userTask = GetUserAsync();
    Task<List<string>> ordersTask = GetOrdersAsync();

    await Task.WhenAll(userTask, ordersTask);

    string user = await userTask;
    List<string> orders = await ordersTask;

    Console.WriteLine(user);
    Console.WriteLine(orders.Count);
}
```

`Task.WhenAll()` completes when all supplied tasks have completed.

This can reduce total waiting time compared with awaiting independent operations one after another.

---

## Sequential vs Concurrent Execution

### Sequential Execution

```csharp
var user = await GetUserAsync();
var orders = await GetOrdersAsync();
```

The second operation starts after the first operation completes.

### Concurrent Execution

```csharp
var userTask = GetUserAsync();
var ordersTask = GetOrdersAsync();

await Task.WhenAll(userTask, ordersTask);
```

Both operations are started before waiting for completion.

Use concurrent execution only when the operations are independent and the underlying services can handle the requests.

---

## Task.Delay() vs Task.Run()

| Method         | Purpose                                       |
| -------------- | --------------------------------------------- |
| `Task.Delay()` | Waits asynchronously for a specified duration |
| `Task.Run()`   | Schedules work on a thread-pool thread        |

### Task.Delay()

```csharp
await Task.Delay(2000);
```

This is useful for asynchronous waiting, retry delays, or simulated I/O delays.

### Task.Run()

```csharp
int result = await Task.Run(() =>
{
    return CalculateLargeValue();
});
```

`Task.Run()` can be useful for CPU-bound work in suitable application types, but it does not make an inherently blocking I/O operation asynchronous.

---

## Exception Handling

Exceptions from awaited tasks can be handled using `try-catch`.

```csharp
public async Task<string> GetDataAsync()
{
    try
    {
        using var client = new HttpClient();

        return await client.GetStringAsync(
            "https://api.example.com/data"
        );
    }
    catch (HttpRequestException ex)
    {
        Console.WriteLine($"Request failed: {ex.Message}");

        return string.Empty;
    }
}
```

Exception handling should be placed at a suitable application boundary, such as a service layer, controller, or global exception handler.

---

## Avoid Blocking Async Code

Avoid using `.Result` or `.Wait()` inside asynchronous code.

### Avoid

```csharp
var result = GetDataAsync().Result;
```

```csharp
GetDataAsync().Wait();
```

### Prefer

```csharp
var result = await GetDataAsync();
```

Blocking calls can cause thread starvation and, in certain synchronization-context scenarios, deadlocks.

---

## ConfigureAwait(false)

In library code, `ConfigureAwait(false)` can be used when the continuation does not need to return to the original synchronization context.

```csharp
var result = await GetDataAsync()
    .ConfigureAwait(false);
```

In modern ASP.NET Core applications, there is generally no traditional request synchronization context, so `ConfigureAwait(false)` is often less necessary than it was in older UI or ASP.NET applications.

---

## Best Practices

* Use `async` and `await` for I/O-bound operations.
* Prefer asynchronous APIs such as `ToListAsync()` and `GetAsync()`.
* Use the `Async` suffix for asynchronous method names.
* Avoid `.Result`, `.Wait()`, and blocking calls.
* Do not use `Task.Run()` for ordinary asynchronous I/O.
* Use `Task.WhenAll()` for independent operations.
* Pass `CancellationToken` when cancellation is required.
* Handle exceptions at the appropriate application boundary.
* Avoid creating a new `HttpClient` for every request in production code.
* Keep asynchronous methods focused and readable.

---

## Interview Questions

### 1. What is async/await?

`async` and `await` are C# features used to write asynchronous, non-blocking code in a readable way.

### 2. What is the difference between Task and Task<T>?

* `Task` represents an operation without a return value.
* `Task<T>` represents an operation that returns a value of type `T`.

### 3. Does await create a new thread?

No. `await` does not automatically create a new thread. It asynchronously waits for a task and allows the current thread to do other work while waiting.

### 4. What is the difference between Task.Delay() and Thread.Sleep()?

* `Task.Delay()` performs an asynchronous delay without blocking the thread.
* `Thread.Sleep()` blocks the current thread.

### 5. What is Task.WhenAll()?

`Task.WhenAll()` waits for multiple tasks to complete and is useful for independent asynchronous operations.

### 6. Why should we avoid .Result and .Wait()?

They block the current thread and may cause thread starvation or deadlocks in certain environments.

### 7. Does async/await improve CPU performance?

Async/await mainly improves responsiveness and scalability for I/O-bound work. It does not automatically make CPU-bound calculations faster.

### 8. How is async/await used with Entity Framework Core?

Entity Framework Core provides asynchronous methods such as:

```csharp
ToListAsync()
FirstOrDefaultAsync()
SaveChangesAsync()
```

These methods help avoid blocking while database operations are running.

---

## Key Takeaway

`async` and `await` make asynchronous programming easier to write and understand.

They help applications remain responsive and scalable by avoiding unnecessary thread blocking during I/O-bound operations.

