# Day 28 — Practical C# Coding Challenges

## 📌 Overview

After learning C# concepts for the previous 27 days, Day 28 focuses on **putting those concepts into practice**.

Practical coding challenges help improve:

* Logical thinking
* Problem-solving skills
* C# coding ability
* LINQ knowledge
* String and collection manipulation
* Exception handling
* Writing clean and readable code
* Interview preparation

The goal is not just to get the correct output, but to understand **how to approach a problem and write maintainable C# code**.

---

# Challenge 1 — Find Duplicate Elements

### ❓ Question

Given a list of integers, find all duplicate elements.

### Example

```text
Input:
[1, 2, 3, 4, 2, 5, 1]

Output:
[1, 2]
```

### ✅ Solution

```csharp
public static List<int> FindDuplicates(List<int> numbers)
{
    return numbers
        .GroupBy(x => x)
        .Where(g => g.Count() > 1)
        .Select(g => g.Key)
        .ToList();
}
```

### How it works

```text
GroupBy()
   ↓
Group identical values
   ↓
Count() > 1
   ↓
Select duplicate keys
```

### Example

```csharp
var numbers = new List<int>
{
    1, 2, 3, 4, 2, 5, 1
};

var duplicates = FindDuplicates(numbers);

Console.WriteLine(string.Join(", ", duplicates));
```

Output:

```text
1, 2
```

---

# Challenge 2 — Fibonacci Series

### ❓ Question

Write a method to generate the first `n` numbers of the Fibonacci series.

### Example

```text
Input:
n = 10

Output:
0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

### ✅ Solution

```csharp
public static List<int> Fibonacci(int n)
{
    var result = new List<int>();

    if (n <= 0)
        return result;

    result.Add(0);

    if (n == 1)
        return result;

    result.Add(1);

    for (int i = 2; i < n; i++)
    {
        result.Add(result[i - 1] + result[i - 2]);
    }

    return result;
}
```

### Example

```csharp
var result = Fibonacci(10);

Console.WriteLine(string.Join(", ", result));
```

Output:

```text
0, 1, 1, 2, 3, 5, 8, 13, 21, 34
```

### Important edge cases

```text
n = 0 → empty list
n = 1 → [0]
n = 2 → [0, 1]
```

---

# Challenge 3 — Reverse a String

### ❓ Question

Reverse a string without using `string.Reverse()`.

### Example

```text
Input:
"Hello"

Output:
"olleH"
```

### ✅ Solution

```csharp
public static string ReverseString(string input)
{
    if (string.IsNullOrEmpty(input))
        return input;

    char[] characters = input.ToCharArray();

    Array.Reverse(characters);

    return new string(characters);
}
```

### Example

```csharp
string result = ReverseString("Hello");

Console.WriteLine(result);
```

Output:

```text
olleH
```

### Another approach

We can also solve this using a loop:

```csharp
public static string ReverseString(string input)
{
    string result = string.Empty;

    for (int i = input.Length - 1; i >= 0; i--)
    {
        result += input[i];
    }

    return result;
}
```

For repeated string concatenation, `StringBuilder` can be preferable for larger inputs.

---

# Challenge 4 — Check Palindrome

### ❓ Question

Check whether a string is a palindrome.

Ignore:

* Spaces
* Special characters
* Letter casing

### Example

```text
Input:
"A man, a plan, a canal: Panama"

Output:
true
```

### ✅ Solution

```csharp
public static bool IsPalindrome(string input)
{
    if (string.IsNullOrWhiteSpace(input))
        return true;

    string processed = new string(
        input
            .Where(char.IsLetterOrDigit)
            .ToArray())
        .ToLowerInvariant();

    string reversed = new string(
        processed.Reverse().ToArray());

    return processed == reversed;
}
```

### Example

```csharp
Console.WriteLine(
    IsPalindrome("A man, a plan, a canal: Panama"));
```

Output:

```text
True
```

### Logic

```text
Original
   ↓
Remove spaces/special characters
   ↓
Convert to lowercase
   ↓
Reverse
   ↓
Compare
```

---

# Challenge 5 — Count Words

### ❓ Question

Count the number of words in a sentence.

### Example

```text
Input:
"Hello world from C#"

Output:
4
```

### ✅ Solution

```csharp
public static int CountWords(string input)
{
    if (string.IsNullOrWhiteSpace(input))
        return 0;

    return input
        .Split(
            ' ',
            StringSplitOptions.RemoveEmptyEntries)
        .Length;
}
```

### Example

```csharp
Console.WriteLine(
    CountWords("Hello world from C#"));
```

Output:

```text
4
```

Using `RemoveEmptyEntries` prevents multiple spaces from being counted as additional words.

---

# Challenge 6 — Check Anagram

### ❓ Question

Check whether two strings are anagrams.

Anagrams contain the same characters with the same frequency, but possibly in a different order.

### Example

```text
Input:
"listen"
"silent"

Output:
true
```

### ✅ Solution

```csharp
public static bool AreAnagrams(
    string first,
    string second)
{
    if (first == null || second == null)
        return false;

    string firstProcessed = new string(
        first
            .Where(char.IsLetterOrDigit)
            .ToArray())
        .ToLowerInvariant();

    string secondProcessed = new string(
        second
            .Where(char.IsLetterOrDigit)
            .ToArray())
        .ToLowerInvariant();

    return firstProcessed
        .OrderBy(c => c)
        .SequenceEqual(
            secondProcessed.OrderBy(c => c));
}
```

### Example

```csharp
Console.WriteLine(
    AreAnagrams("listen", "silent"));
```

Output:

```text
True
```

### Logic

```text
"listen"
   ↓
"eilnst"

"silent"
   ↓
"eilnst"

Compare
   ↓
True
```

---

# Challenge 7 — Simple Calculator

### ❓ Question

Create a calculator that supports:

* Addition
* Subtraction
* Multiplication
* Division

### Example

```text
Input:
10 + 5

Output:
15
```

### ✅ Solution

```csharp
public static double Calculate(
    double a,
    double b,
    char operation)
{
    return operation switch
    {
        '+' => a + b,
        '-' => a - b,
        '*' => a * b,

        '/' when b != 0 => a / b,

        '/' => throw new DivideByZeroException(
            "Cannot divide by zero."),

        _ => throw new ArgumentException(
            "Invalid operator.")
    };
}
```

### Example

```csharp
Console.WriteLine(
    Calculate(10, 5, '+'));

Console.WriteLine(
    Calculate(10, 5, '*'));
```

Output:

```text
15
50
```

### Concepts used

* Method
* Switch expression
* Exception handling
* Input validation

---

# Challenge 8 — Serialize and Deserialize JSON

### ❓ Question

Create a list of employees and:

1. Serialize it into JSON.
2. Deserialize the JSON back into objects.

### Employee class

```csharp
public class Employee
{
    public int Id { get; set; }

    public string Name { get; set; } = string.Empty;

    public string Department { get; set; } = string.Empty;
}
```

### Serialize

```csharp
using System.Text.Json;

public static string SerializeEmployees(
    List<Employee> employees)
{
    return JsonSerializer.Serialize(employees);
}
```

### Deserialize

```csharp
public static List<Employee> DeserializeEmployees(
    string json)
{
    return JsonSerializer.Deserialize<List<Employee>>(json)
           ?? new List<Employee>();
}
```

### Example

```csharp
var employees = new List<Employee>
{
    new Employee
    {
        Id = 1,
        Name = "Ashish",
        Department = "IT"
    },
    new Employee
    {
        Id = 2,
        Name = "Rahul",
        Department = "HR"
    }
};

string json = SerializeEmployees(employees);

Console.WriteLine(json);

var result = DeserializeEmployees(json);
```

### Example JSON

```json
[
  {
    "Id": 1,
    "Name": "Ashish",
    "Department": "IT"
  },
  {
    "Id": 2,
    "Name": "Rahul",
    "Department": "HR"
  }
]
```

### Concepts used

* Classes
* Collections
* Generics
* JSON serialization
* JSON deserialization
* `System.Text.Json`

---

# Challenge 9 — Find the Largest Number

### ❓ Question

Find the largest number from an integer list.

### Example

```text
Input:
[10, 25, 5, 40, 15]

Output:
40
```

### ✅ Solution using LINQ

```csharp
public static int FindLargest(List<int> numbers)
{
    return numbers.Max();
}
```

### Without LINQ

```csharp
public static int FindLargest(List<int> numbers)
{
    if (numbers == null || numbers.Count == 0)
        throw new ArgumentException(
            "List cannot be empty.");

    int largest = numbers[0];

    foreach (int number in numbers)
    {
        if (number > largest)
        {
            largest = number;
        }
    }

    return largest;
}
```

This is a useful interview exercise because it tests basic looping and comparison logic.

---

# Challenge 10 — Count Character Frequency

### ❓ Question

Count how many times each character appears in a string.

### Example

```text
Input:
"hello"

Output:
h = 1
e = 1
l = 2
o = 1
```

### ✅ Solution

```csharp
public static Dictionary<char, int> CharacterFrequency(
    string input)
{
    var result = new Dictionary<char, int>();

    foreach (char character in input)
    {
        if (result.ContainsKey(character))
        {
            result[character]++;
        }
        else
        {
            result[character] = 1;
        }
    }

    return result;
}
```

### Example

```csharp
var result = CharacterFrequency("hello");

foreach (var item in result)
{
    Console.WriteLine(
        $"{item.Key} = {item.Value}");
}
```

---

# 🧠 How to Approach Coding Challenges

Before writing code, follow these steps:

### 1. Understand the problem

Identify:

* Input
* Output
* Rules
* Constraints

### 2. Think about edge cases

Examples:

```text
null
empty input
single element
duplicate values
negative numbers
invalid input
division by zero
```

### 3. Start with a simple solution

Don't immediately try to write the most complicated solution.

First make it:

**Correct → Readable → Efficient**

### 4. Test with multiple inputs

Example:

```text
Normal input
Empty input
Invalid input
Boundary input
Large input
```

### 5. Think about complexity

Ask:

* What is the time complexity?
* What is the space complexity?
* Can the solution be improved?

---

# 💡 Interview Tips

When solving a coding problem in an interview:

```text
Understand
    ↓
Explain approach
    ↓
Write code
    ↓
Test with example
    ↓
Check edge cases
    ↓
Discuss complexity
```

Don't start coding immediately.

Explain your approach first.

---

# 📊 Useful C# Features Practiced

| Concept                   | Used In                        |
| ------------------------- | ------------------------------ |
| `List<T>`                 | Duplicate/Fibonacci challenges |
| LINQ                      | Duplicates/Palindrome/Anagram  |
| Loops                     | Fibonacci/Frequency            |
| `Dictionary<TKey,TValue>` | Character frequency            |
| String manipulation       | Reverse/Palindrome/Anagram     |
| Exception handling        | Calculator                     |
| Switch expression         | Calculator                     |
| Generics                  | Collections                    |
| `System.Text.Json`        | JSON challenge                 |
| Classes                   | Employee/JSON challenge        |

---

# 🎯 Key Takeaway

Practical coding challenges help turn **C# knowledge into problem-solving ability**.

The important skill is not memorizing solutions.

It is learning how to:

* Understand a problem
* Break it into smaller steps
* Handle edge cases
* Select the right C# features
* Write clean code
* Test your solution
* Think about performance

> **Practice small problems regularly. It improves logic, coding confidence, and interview readiness.**

---


