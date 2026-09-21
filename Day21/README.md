# Day 21 — LINQ in C#

## Overview

LINQ stands for **Language Integrated Query**.

It is a feature of C# that allows developers to query and manipulate data from different sources using a consistent and readable syntax.

LINQ can be used with:

* Collections
* Arrays
* Lists
* Dictionaries
* XML
* Databases
* Entity Framework Core

LINQ helps developers write cleaner code by reducing repetitive loops and conditions.

---

## Why Use LINQ?

The main benefits of LINQ are:

* Cleaner and more readable code
* Type-safe queries
* Less repetitive code
* Easy filtering, sorting, and grouping
* Consistent syntax across different data sources
* Support for deferred execution
* Integration with Entity Framework Core

---

## Basic LINQ Example

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        var numbers = new List<int>
        {
            1, 2, 3, 4, 5
        };

        var evenNumbers = numbers
            .Where(n => n % 2 == 0)
            .ToList();

        foreach (var number in evenNumbers)
        {
            Console.WriteLine(number);
        }
    }
}
```

### Output

```text
2
4
```

---

## Common LINQ Operators

| Operator              | Purpose                                        |
| --------------------- | ---------------------------------------------- |
| `Where()`             | Filters data based on a condition              |
| `Select()`            | Projects data into a new form                  |
| `OrderBy()`           | Sorts data in ascending order                  |
| `OrderByDescending()` | Sorts data in descending order                 |
| `ThenBy()`            | Applies secondary ascending sorting            |
| `GroupBy()`           | Groups data using a key                        |
| `First()`             | Returns the first element                      |
| `FirstOrDefault()`    | Returns the first element or default value     |
| `Any()`               | Checks whether any element matches a condition |
| `All()`               | Checks whether all elements match a condition  |
| `Count()`             | Counts elements                                |
| `ToList()`            | Executes the query and creates a list          |

---

## Filtering with Where()

`Where()` filters data based on a condition.

```csharp
var ages = new List<int>
{
    15, 18, 21, 25, 30
};

var adults = ages
    .Where(age => age >= 18)
    .ToList();
```

### Result

```text
18, 21, 25, 30
```

Multiple conditions can also be used:

```csharp
var selectedAges = ages
    .Where(age => age >= 18 && age <= 25)
    .ToList();
```

---

## Projection with Select()

`Select()` transforms each element into a new form.

```csharp
var names = new List<string>
{
    "Ashish",
    "Rahul",
    "Priya"
};

var nameLengths = names
    .Select(name => name.Length)
    .ToList();
```

It can also project objects into selected properties:

```csharp
var employeeNames = employees
    .Select(employee => employee.Name)
    .ToList();
```

Anonymous objects can be created using `Select()`:

```csharp
var employeeDetails = employees
    .Select(employee => new
    {
        employee.Name,
        employee.Department
    })
    .ToList();
```

---

## Sorting with OrderBy()

```csharp
var sortedEmployees = employees
    .OrderBy(employee => employee.Name)
    .ToList();
```

Descending sorting:

```csharp
var highestPaidEmployees = employees
    .OrderByDescending(employee => employee.Salary)
    .ToList();
```

Multiple sorting conditions:

```csharp
var sortedEmployees = employees
    .OrderBy(employee => employee.Department)
    .ThenBy(employee => employee.Name)
    .ToList();
```

---

## Grouping with GroupBy()

`GroupBy()` creates groups based on a selected key.

```csharp
var employeesByDepartment = employees
    .GroupBy(employee => employee.Department);

foreach (var group in employeesByDepartment)
{
    Console.WriteLine($"Department: {group.Key}");

    foreach (var employee in group)
    {
        Console.WriteLine(employee.Name);
    }
}
```

This is useful for grouping employees by department, products by category, or doctors by specialization.

---

## FirstOrDefault()

`FirstOrDefault()` returns the first matching element.

```csharp
var employee = employees
    .FirstOrDefault(employee => employee.Name == "Ashish");
```

If no matching element is found, it returns the default value.

For reference types, the default value is usually `null`.

---

## Any(), All(), and Count()

### Any()

Checks whether at least one element matches a condition.

```csharp
bool hasSeniorEmployee = employees
    .Any(employee => employee.Experience > 5);
```

### All()

Checks whether every element matches a condition.

```csharp
bool areAllAdults = ages
    .All(age => age >= 18);
```

### Count()

Returns the number of elements.

```csharp
int employeeCount = employees.Count();
```

It can also count matching elements:

```csharp
int experiencedEmployees = employees
    .Count(employee => employee.Experience >= 3);
```

---

## Deferred and Immediate Execution

### Deferred Execution

The query is not executed when it is created. It is executed when the result is enumerated.

Common operators that use deferred execution:

* `Where()`
* `Select()`
* `OrderBy()`
* `GroupBy()`

```csharp
var query = numbers
    .Where(number => number > 2);
```

The query runs when it is iterated:

```csharp
foreach (var number in query)
{
    Console.WriteLine(number);
}
```

### Immediate Execution

Immediate execution happens when the query is executed immediately.

Common operators include:

* `ToList()`
* `ToArray()`
* `Count()`
* `First()`
* `FirstOrDefault()`

```csharp
var result = numbers
    .Where(number => number > 2)
    .ToList();
```

---

## LINQ with Entity Framework Core

LINQ is commonly used with Entity Framework Core to query databases.

```csharp
var doctors = context.Doctors
    .Where(doctor => doctor.Specialization == "Cardiology")
    .OrderBy(doctor => doctor.Name)
    .ToList();
```

Entity Framework Core translates many LINQ queries into SQL and executes them against the database.

For asynchronous database queries:

```csharp
var doctors = await context.Doctors
    .Where(doctor => doctor.Specialization == "Cardiology")
    .OrderBy(doctor => doctor.Name)
    .ToListAsync();
```

Required namespace:

```csharp
using Microsoft.EntityFrameworkCore;
```

---

## Method Syntax vs Query Syntax

### Method Syntax

```csharp
var result = numbers
    .Where(number => number % 2 == 0)
    .OrderBy(number => number)
    .ToList();
```

### Query Syntax

```csharp
var result =
    (from number in numbers
     where number % 2 == 0
     orderby number
     select number)
    .ToList();
```

Both approaches can produce the same result.

---

## Practical Example

```csharp
var employees = new List<Employee>
{
    new Employee { Name = "Ashish", Department = "IT", Salary = 45000 },
    new Employee { Name = "Rahul", Department = "HR", Salary = 40000 },
    new Employee { Name = "Priya", Department = "IT", Salary = 55000 }
};

var itEmployees = employees
    .Where(employee => employee.Department == "IT")
    .OrderByDescending(employee => employee.Salary)
    .Select(employee => new
    {
        employee.Name,
        employee.Salary
    })
    .ToList();
```

This query:

1. Filters employees from the IT department.
2. Sorts them by salary in descending order.
3. Selects only the name and salary.
4. Converts the result into a list.

---

## Interview Questions

### 1. What is LINQ?

LINQ is a C# feature used to query data from different sources using a consistent syntax.

### 2. What is the difference between `Where()` and `Select()`?

* `Where()` filters data.
* `Select()` transforms or projects data.

### 3. What is deferred execution?

Deferred execution means a LINQ query is not executed until its result is enumerated.

### 4. What is the difference between `First()` and `FirstOrDefault()`?

* `First()` throws an exception if no element is found.
* `FirstOrDefault()` returns the default value if no element is found.

### 5. Can LINQ be used with databases?

Yes. Entity Framework Core uses LINQ to build and execute database queries.

### 6. What does `ToList()` do?

`ToList()` executes the query immediately and stores the results in a list.

### 7. What is the difference between `Any()` and `Count()`?

* `Any()` checks whether matching elements exist.
* `Count()` calculates the number of elements.

### 8. What is the difference between `IEnumerable<T>` and `IQueryable<T>`?

* `IEnumerable<T>` is generally used for in-memory data.
* `IQueryable<T>` represents a query that can be translated and executed by a provider, such as Entity Framework Core.

---

## Key Takeaway

LINQ makes data querying simple, readable, and powerful.

It allows developers to filter, sort, group, transform, and analyze data with less code. LINQ is especially important when working with collections and Entity Framework Core.


