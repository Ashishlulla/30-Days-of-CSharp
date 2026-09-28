# Day 29 — Real-World C# & .NET Scenarios

## 📌 Overview

Learning C# concepts individually is important, but real-world development requires combining multiple concepts to solve business problems.

Day 29 focuses on common scenarios encountered while building modern **C# and .NET applications**.

The goal is to understand how different technologies and concepts work together in a real application.

---

# 1. Building a RESTful API

### ❓ Scenario

A frontend application needs to retrieve employee data from a backend server.

ASP.NET Core Web API can expose an HTTP endpoint that returns JSON data.

### ✅ Solution

```csharp
[ApiController]
[Route("api/[controller]")]
public class EmployeesController : ControllerBase
{
    [HttpGet]
    public IActionResult GetEmployees()
    {
        var employees = new[]
        {
            new { Id = 1, Name = "Ashish" },
            new { Id = 2, Name = "Rahul" }
        };

        return Ok(employees);
    }
}
```

### Request

```http
GET /api/employees
```

### Response

```json
[
  {
    "id": 1,
    "name": "Ashish"
  },
  {
    "id": 2,
    "name": "Rahul"
  }
]
```

### Real-world flow

```text
Angular / React
      ↓
HTTP Request
      ↓
ASP.NET Core API
      ↓
Service
      ↓
Repository / EF Core
      ↓
SQL Server
```

---

# 2. Working with Entity Framework Core

### ❓ Scenario

Retrieve all employees belonging to the IT department.

### ✅ Solution

```csharp
var employees = await context.Employees
    .Where(e => e.Department.Name == "IT")
    .ToListAsync();
```

EF Core translates the LINQ query into SQL and executes it against the database.

### Why EF Core?

* Database access
* LINQ queries
* CRUD operations
* Relationships
* Migrations
* Change tracking

---

# 3. Authentication & Authorization

### ❓ Scenario

Only administrators should access an admin-only API endpoint.

### ✅ Solution

```csharp
[Authorize(Roles = "Admin")]
[HttpGet("admin-data")]
public IActionResult GetAdminData()
{
    return Ok("Only administrators can access this.");
}
```

### Authentication vs Authorization

| Authentication   | Authorization        |
| ---------------- | -------------------- |
| Who are you?     | What can you access? |
| Login / Identity | Permissions / Roles  |
| JWT / Cookies    | Roles / Policies     |

### Example

```text
User Login
    ↓
Authentication
    ↓
JWT Token
    ↓
Authorization
    ↓
Check Role
    ↓
Allow / Deny Access
```

---

# 4. Sending Emails

### ❓ Scenario

An application needs to send an email after a user registers or when an appointment is created.

### ✅ Example

```csharp
await emailService.SendEmailAsync(
    "user@example.com",
    "Welcome",
    "Welcome to our application!");
```

A real application may use:

* SMTP
* Brevo
* SendGrid
* Microsoft Graph
* Other email providers

### Typical use cases

```text
Registration
Password Reset
Appointment Confirmation
Notifications
Reports
```

Email functionality is usually placed behind an interface:

```csharp
public interface IEmailService
{
    Task SendEmailAsync(
        string to,
        string subject,
        string body);
}
```

This keeps the business logic independent from the actual email provider.

---

# 5. Generating Reports

### ❓ Scenario

A business application needs to generate an Excel report containing employee information.

### Example using ClosedXML

```csharp
using var workbook = new XLWorkbook();

var worksheet = workbook.Worksheets.Add("Employees");

worksheet.Cell(1, 1).Value = "Id";
worksheet.Cell(1, 2).Value = "Name";
worksheet.Cell(1, 3).Value = "Department";

worksheet.Cell(2, 1).Value = 1;
worksheet.Cell(2, 2).Value = "Ashish";
worksheet.Cell(2, 3).Value = "IT";

workbook.SaveAs("employees.xlsx");
```

### Real-world reports

* Employee reports
* Sales reports
* Invoices
* Billing reports
* Business insights
* Attendance reports

---

# 6. Background Services

### ❓ Scenario

An application needs to perform a task automatically every few hours.

Examples:

* Send scheduled emails
* Clean temporary data
* Generate reports
* Synchronize data
* Process queued jobs

### ✅ Example

```csharp
public class EmailBackgroundService
    : BackgroundService
{
    protected override async Task ExecuteAsync(
        CancellationToken stoppingToken)
    {
        while (!stoppingToken.IsCancellationRequested)
        {
            await SendDailyReports();

            await Task.Delay(
                TimeSpan.FromHours(24),
                stoppingToken);
        }
    }

    private async Task SendDailyReports()
    {
        // Send reports
    }
}
```

Register it:

```csharp
builder.Services.AddHostedService<
    EmailBackgroundService>();
```

### Important

A background service should respect the `CancellationToken` so it can shut down gracefully.

---

# 7. Logging & Monitoring

### ❓ Scenario

A production application throws an unexpected error.

Instead of only showing an error to the user, we need useful information for developers.

### Example

```csharp
_logger.LogInformation(
    "User {UserId} logged in.",
    userId);
```

For errors:

```csharp
try
{
    await ProcessOrder();
}
catch (Exception ex)
{
    _logger.LogError(
        ex,
        "Error while processing order.");

    throw;
}
```

### Common log levels

```text
Trace
Debug
Information
Warning
Error
Critical
```

### Why logging matters?

```text
Application
    ↓
Logs
    ↓
Investigation
    ↓
Root Cause
    ↓
Fix
```

---

# 8. Caching

### ❓ Scenario

A department list is requested frequently but changes rarely.

Calling the database every time is unnecessary.

We can cache the result.

### Memory Cache example

```csharp
if (!_cache.TryGetValue(
    "departments",
    out List<Department>? departments))
{
    departments = await GetDepartments();

    _cache.Set(
        "departments",
        departments,
        TimeSpan.FromMinutes(30));
}
```

### Benefits

* Faster responses
* Fewer database queries
* Reduced server/database load

### Types

```text
In-Memory Cache
      ↓
Single application instance

Distributed Cache
      ↓
Shared between application instances
      ↓
Redis
```

---

# 9. Dependency Injection

### ❓ Scenario

A service needs a repository to access employee data.

Instead of creating the repository manually:

```csharp
var repository = new EmployeeRepository();
```

Use Dependency Injection.

### Interface

```csharp
public interface IEmployeeRepository
{
    Task<Employee?> GetByIdAsync(int id);
}
```

### Service

```csharp
public class EmployeeService
{
    private readonly IEmployeeRepository _repository;

    public EmployeeService(
        IEmployeeRepository repository)
    {
        _repository = repository;
    }

    public async Task<Employee?> GetEmployee(int id)
    {
        return await _repository.GetByIdAsync(id);
    }
}
```

### Registration

```csharp
builder.Services.AddScoped<
    IEmployeeRepository,
    EmployeeRepository>();
```

### Benefits

* Loose coupling
* Easier testing
* Better maintainability
* Clear separation of responsibilities

---

# 10. LINQ for Data Processing

### ❓ Scenario

Find the top 5 employees with the highest salary.

### ✅ Solution

```csharp
var topEmployees = employees
    .Where(e => e.Salary > 0)
    .OrderByDescending(e => e.Salary)
    .Take(5)
    .ToList();
```

### Another example

Find employees from the IT department:

```csharp
var itEmployees = employees
    .Where(e => e.Department == "IT")
    .ToList();
```

LINQ provides a clean way to filter, sort, group and transform data.

---

# 11. Combining Everything in a Real Application

A typical .NET application may combine all these concepts:

```text
              Client
                ↓
          ASP.NET Core API
                ↓
       Authentication / JWT
                ↓
           Controller
                ↓
            Service
                ↓
       Dependency Injection
                ↓
         Repository / EF Core
                ↓
            SQL Server
```

Alongside this:

```text
Logging ────────────────→ Monitor application

Caching ────────────────→ Improve performance

Background Service ─────→ Scheduled tasks

Email Service ──────────→ Notifications

Reports ────────────────→ PDF / Excel output
```

---

# 12. Real-World Scenario — Employee Management

### ❓ Requirement

Build an employee management API with:

* Employee CRUD
* Authentication
* Role-based authorization
* SQL Server database
* Logging
* Email notifications

### Possible architecture

```text
Web / Angular
      ↓
ASP.NET Core Web API
      ↓
Controller
      ↓
Application Service
      ↓
Repository
      ↓
EF Core
      ↓
SQL Server
```

Additional services:

```text
IEmailService
ILogger
ICacheService
```

This demonstrates how multiple C# and .NET concepts work together rather than independently.

---

# 13. Real-World Scenario — Error Handling

### ❓ Scenario

A database operation fails.

A good application should:

1. Catch the appropriate exception where it can be meaningfully handled.
2. Log the exception.
3. Avoid exposing internal details to users.
4. Return an appropriate response.

### Example

```csharp
try
{
    var employee =
        await employeeService.GetEmployee(id);

    return Ok(employee);
}
catch (Exception ex)
{
    logger.LogError(
        ex,
        "Error retrieving employee {EmployeeId}",
        id);

    return StatusCode(
        500,
        "An unexpected error occurred.");
}
```

In larger applications, centralized exception-handling middleware is often preferable to putting broad `try-catch` blocks in every controller.

---

# 14. Real-World Scenario — Performance

### ❓ Problem

An API is taking too long to return employee data.

### Possible investigation

```text
Slow API
   ↓
Check Logs
   ↓
Check Database Query
   ↓
Check LINQ / EF Core
   ↓
Check Network
   ↓
Check Caching
   ↓
Optimize
```

Possible improvements:

* Use proper database indexes
* Avoid unnecessary data loading
* Use projection
* Use pagination
* Use asynchronous operations
* Add caching where appropriate
* Avoid unnecessary database calls

### Example projection

Instead of retrieving an entire entity:

```csharp
var employees = await context.Employees
    .Select(e => new
    {
        e.Id,
        e.Name,
        e.Department.Name
    })
    .ToListAsync();
```

Only the required data is selected.

---

# 15. Useful .NET Technologies

| Technology            | Common Use              |
| --------------------- | ----------------------- |
| ASP.NET Core          | Web applications & APIs |
| Entity Framework Core | Database access         |
| SQL Server            | Relational database     |
| JWT                   | Authentication          |
| Dependency Injection  | Managing dependencies   |
| Serilog               | Structured logging      |
| IMemoryCache          | In-memory caching       |
| Redis                 | Distributed caching     |
| BackgroundService     | Background processing   |
| ClosedXML             | Excel reports           |
| QuestPDF              | PDF generation          |
| SMTP / Email APIs     | Email communication     |
| LINQ                  | Data querying           |

---

# 16. Interview Questions

### Q1. What is the difference between authentication and authorization?

**Authentication** verifies who the user is.

**Authorization** determines what the authenticated user is allowed to access.

---

### Q2. Why is Dependency Injection useful?

It reduces tight coupling and makes components easier to test, replace and maintain.

---

### Q3. Why use async/await in Web APIs?

Asynchronous I/O allows the application to handle waiting operations such as database or network calls without unnecessarily blocking threads.

---

### Q4. What is caching?

Caching stores frequently used data temporarily so future requests can retrieve it faster.

---

### Q5. What is the difference between memory cache and distributed cache?

**Memory cache** is stored inside a particular application instance.

**Distributed cache** can be shared between multiple application instances.

---

### Q6. Why is logging important?

Logging provides information about application behavior, errors and events, which helps with troubleshooting and monitoring.

---

### Q7. What is a BackgroundService?

`BackgroundService` is a base class used to implement long-running background tasks in .NET applications.

---

### Q8. Why use LINQ?

LINQ provides a readable way to filter, sort, group, project and query data.

---

### Q9. Why should business logic not be placed directly in controllers?

Keeping business logic in services improves separation of concerns, testability and maintainability.

---

### Q10. How would you investigate a slow API?

A practical approach is:

```text
Check logs
    ↓
Measure API response time
    ↓
Inspect database queries
    ↓
Check indexes
    ↓
Check data being returned
    ↓
Check external services
    ↓
Consider caching
    ↓
Optimize and measure again
```

---

# 🎯 Key Takeaway

Real-world .NET development is about **combining multiple concepts to solve business problems**.

A production application may use:

```text
ASP.NET Core
     +
EF Core
     +
SQL Server
     +
Authentication
     +
Dependency Injection
     +
Logging
     +
Caching
     +
Background Services
     +
LINQ
     +
Reports / Email
```

Understanding how these pieces work together is an important step from **learning C# concepts to developing real applications**.

> **Learn the concept → Understand the problem → Apply the right solution.**

---

