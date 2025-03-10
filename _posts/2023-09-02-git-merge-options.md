---
date: 2023-08-31 00:30:00 +0530
layout: post
title: Singleton Pattern
subtitle: different approaches for singleton pattern
description: Different approaches to implement singleton pattern
image: /assets/img/singleton.jpg
optimized_image: /assets/img/singleton.jpg
category: code
tags:
  - code
  - design
author: ankurmalviya1618
my_prop: 'nil'
---
# Singleton Design Pattern in Java: A Complete Guide

The Singleton Design Pattern is one of the most widely used creational design patterns in Java. It ensures that a class has only one instance and provides a global access point to that instance.

In this blog, we'll explore different ways to implement the Singleton Pattern and discuss their pros and cons.

## Why Singleton?
The Singleton pattern is useful when:

You need a single shared resource, like a Database connection, Logger, Configuration manager, or Cache.
You want to control concurrent access to a shared resource.

## Implementing Singleton in Java
There are multiple ways to implement the Singleton pattern in Java. Let’s explore them:

<hr>

## 1. Eager Loading Singleton
In Eager Loading, the instance is created at the time of class loading, ensuring thread safety.

### Implementation

```java
public class EagerSingleton {
    // Instance is created at class loading time
    private static final EagerSingleton INSTANCE = new EagerSingleton();

    // Private constructor prevents instantiation
    private EagerSingleton() {}

    // Public method to provide access to the instance
    public static EagerSingleton getInstance() {
        return INSTANCE;
    }
}
```

### Pros:
✅ Thread-safe without synchronization
✅ Simple and easy to implement

### Cons:
❌ Instance is created even if it is never used (wasted memory)

## 2. Lazy Loading Singleton (Double-Checked Locking with volatile)
In Lazy Loading, the instance is created only when it is first requested.
To ensure thread safety without performance overhead, we use Double-Checked Locking with volatile.

### Implementation

```java
public class LazySingleton {
    // Volatile ensures visibility across threads
    private static volatile LazySingleton instance;

    // Private constructor prevents instantiation
    private LazySingleton() {}

    // Double-checked locking for thread safety
    public static LazySingleton getInstance() {
        if (instance == null) { // First check (no locking)
            synchronized (LazySingleton.class) {
                if (instance == null) { // Second check (with locking)
                    instance = new LazySingleton();
                }
            }
        }
        return instance;
    }
}
```

### Pros:
✅ Thread-safe without major performance issues
✅ Instance is created only when needed

### Cons:
❌ Slightly complex due to synchronization logic

## 3. Singleton Using Static Inner Class
This approach leverages lazy loading and thread safety without synchronization overhead. The instance is created only when the inner class is loaded.

### Implementation

```java
public class StaticInnerSingleton {
    // Private constructor prevents instantiation
    private StaticInnerSingleton() {}

    // Static inner class - instance is created only when accessed
    private static class SingletonHelper {
        private static final StaticInnerSingleton INSTANCE = new StaticInnerSingleton();
    }

    // Public method to get the instance
    public static StaticInnerSingleton getInstance() {
        return SingletonHelper.INSTANCE;
    }
}
```

### Pros:
✅ Thread-safe without synchronization
✅ Lazy loading (instance is created only when getInstance() is called)
✅ Simple and efficient

### Cons:
❌ Does not allow exception handling in instance creation

## 4. Singleton Using Enum (Best Approach)
The Enum Singleton approach is the best way to implement a Singleton in Java because:

- It is inherently thread-safe.
- It prevents multiple instances even in complex serialization scenarios.
- It protects against reflection attacks.

### Implementation

```java
public enum EnumSingleton {
    INSTANCE;

    public void showMessage() {
        System.out.println("Hello from Enum Singleton!");
    }
}
```

### Usage
```java
EnumSingleton.INSTANCE.showMessage();
```

### Pros:
✅ Thread-safe without synchronization
✅ Serialization-safe by default
✅ Reflection-safe (prevents instantiation via reflection)

### Cons:
❌ Cannot extend another class (since enums cannot extend classes)

## Comparison of Different Singleton Approaches

| Approach                      | Thread-Safe | Lazy Loading | Performance  | Best Use Case |
|--------------------------------|------------|--------------|-------------|---------------|
| **Eager Loading**              | ✅ Yes      | ❌ No        | ⭐⭐⭐ Fast    | Simple singletons where early instantiation is fine |
| **Lazy (Double-Checked Locking)** | ✅ Yes      | ✅ Yes       | ⭐⭐ Moderate | When instance creation is expensive |
| **Static Inner Class**         | ✅ Yes      | ✅ Yes       | ⭐⭐⭐ Fast    | Best for most use cases |
| **Enum Singleton**             | ✅ Yes      | ✅ Yes       | ⭐⭐⭐ Fast    | Best for enterprise applications |


## Which Singleton Approach Should You Use?
- Use Eager Loading if the instance is lightweight and will always be used.
- Use Lazy Initialization (Double-Checked Locking) when the instance is expensive to create.
- Use Static Inner Class for an efficient, thread-safe, and simple implementation.
- Use Enum Singleton when you need serialization safety and protection against reflection.

## Conclusion
The Singleton Design Pattern is a powerful tool when used correctly. Different implementations have their own trade-offs, so choosing the right approach depends on your application needs.

If you want the simplest and safest approach, Enum Singleton is the best choice.

> Happy Coding :)

