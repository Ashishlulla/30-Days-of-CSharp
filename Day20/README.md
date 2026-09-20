# 📘 Day 20 — Delegates & Events in C#

**Delegates** and **events** are important C# features used for method references, callbacks, and event-driven programming.

A delegate can reference a method with a compatible signature, while an event provides a controlled notification mechanism.

## 📣 Why Do We Use Delegates & Events?

They help us:

* Pass methods as parameters.
* Implement callbacks.
* Build loosely coupled applications.
* Notify other parts of a program when something happens.
* Support event-driven programming.

## 1. What Is a Delegate?

A delegate is a type-safe reference to a method.

It defines the method signature that a referenced method must match.

### Example

```csharp
public delegate void MessageHandler(string message);
```

This delegate can reference methods that accept a `string` parameter and return `void`.

## 2. Using a Delegate

```csharp
public delegate void MessageHandler(string message);

class Program
{
    static void ShowMessage(string message)
    {
        Console.WriteLine(message);
    }

    static void Main()
    {
        MessageHandler handler = ShowMessage;

        handler("Hello from Delegate!");
    }
}
```

**Output:**

```text
Hello from Delegate!
```

The delegate holds a reference to the `ShowMessage` method.

## 3. Multicast Delegates

A delegate can reference multiple methods.

Use `+=` to add methods and `-=` to remove them.

```csharp
public delegate void MessageHandler(string message);

class Program
{
    static void ShowMessage1(string message)
    {
        Console.WriteLine("Message 1: " + message);
    }

    static void ShowMessage2(string message)
    {
        Console.WriteLine("Message 2: " + message);
    }

    static void Main()
    {
        MessageHandler handler = ShowMessage1;

        handler += ShowMessage2;

        handler("Hello!");
    }
}
```

**Output:**

```text
Message 1: Hello!
Message 2: Hello!
```

The methods are invoked in the order they were added.

## 4. Built-in Delegates

C# provides built-in generic delegates.

| Delegate        | Purpose                               |
| --------------- | ------------------------------------- |
| `Action`        | Represents a method returning `void`  |
| `Func<TResult>` | Represents a method returning a value |
| `Predicate<T>`  | Represents a method returning `bool`  |

### Example

```csharp
Action<string> print = message =>
    Console.WriteLine(message);

Func<int, int, int> add = (a, b) => a + b;

Predicate<int> isEven = number => number % 2 == 0;

print("Hello C#");

Console.WriteLine(add(10, 20));

Console.WriteLine(isEven(4));
```

**Output:**

```text
Hello C#
30
True
```

## 5. What Is an Event?

An event is a member that enables a class to notify other objects when something happens.

Events are built on delegates and are commonly used in event-driven programming.

Examples:

* Button clicks.
* Notifications.
* Logging.
* Data updates.
* Status changes.

## 6. Event Example — Publisher and Subscriber

A publisher raises an event, and subscribers respond to it.

```csharp
class Publisher
{
    public event Action<string>? MessageReceived;

    public void SendMessage(string message)
    {
        MessageReceived?.Invoke(message);
    }
}
```

Subscriber:

```csharp
class Subscriber
{
    public void Subscribe(Publisher publisher)
    {
        publisher.MessageReceived += HandleMessage;
    }

    private void HandleMessage(string message)
    {
        Console.WriteLine("Received: " + message);
    }
}
```

Usage:

```csharp
Publisher publisher = new Publisher();

Subscriber subscriber = new Subscriber();

subscriber.Subscribe(publisher);

publisher.SendMessage("Hello Subscriber!");
```

**Output:**

```text
Received: Hello Subscriber!
```

The subscriber receives the notification when the publisher raises the event.

## 7. Event Access Control

The class that declares an event can raise it.

Other classes can subscribe or unsubscribe, but cannot directly invoke the event.

```csharp
publisher.MessageReceived += HandleMessage;
```

This is one of the important differences between a delegate and an event.

## 8. EventHandler Pattern

.NET commonly uses `EventHandler` and `EventHandler<TEventArgs>` for events.

```csharp
class Order
{
    public event EventHandler? OrderPlaced;

    public void PlaceOrder()
    {
        Console.WriteLine("Order placed.");

        OrderPlaced?.Invoke(this, EventArgs.Empty);
    }
}
```

This follows the conventional .NET event pattern.

## 9. Delegate vs Event

| Feature          | Delegate                                           | Event                                        |
| ---------------- | -------------------------------------------------- | -------------------------------------------- |
| Purpose          | Holds method references                            | Notifies subscribers                         |
| Invocation       | Can be invoked by code with access to the delegate | Can only be raised within the declaring type |
| Multiple methods | Supported                                          | Multiple subscribers supported               |
| Common use       | Callbacks, passing methods                         | Event-driven notifications                   |
| Access           | More direct control                                | Controlled subscription and notification     |

## 10. Real-World Example — Notification System

In a .NET application, an event can notify multiple subscribers when an action occurs.

```text
                Publisher
                    |
              Event Raised
              /     |     \
             /      |      \
          Email     SMS    Logging
        Subscriber Subscriber Subscriber
```

For example, when an appointment is created:

* Email service sends a confirmation.
* Notification service creates an in-app notification.
* Logging service records the activity.

Each subscriber can respond independently to the same event.

## 11. Best Practices

* Use events for notifications.
* Use `EventHandler` for conventional .NET events.
* Keep delegate signatures simple.
* Unsubscribe from long-lived publishers when appropriate.
* Avoid exposing delegates directly when an event provides safer access.
* Use meaningful event names, such as `OrderPlaced` or `AppointmentCreated`.

## 🧠 Interview Questions

### Q1. What is a delegate in C#?

A delegate is a type-safe reference to a method with a compatible signature.

### Q2. What is an event?

An event provides a controlled mechanism for notifying subscribers when something happens.

### Q3. Can a delegate reference multiple methods?

**Yes.** A multicast delegate can reference multiple methods.

### Q4. What is the difference between a delegate and an event?

A delegate can be invoked by code with access to it, while an event can only be raised from within its declaring type.

### Q5. What are `Action`, `Func`, and `Predicate`?

They are built-in generic delegate types used for void-returning methods, value-returning methods, and Boolean conditions.

### Q6. What is the publisher-subscriber pattern?

The publisher raises an event, and subscribers register handlers to respond to that event.

## ⭐ Key Takeaway

> **Delegates let us reference methods, while events provide a safe and flexible way to notify other parts of an application.**

---

