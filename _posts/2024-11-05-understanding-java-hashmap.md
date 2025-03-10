---
date: 2024-11-05 15:40:00 +0530
layout: post
title: Mastering Java HashMap - An In Depth Guide 
subtitle: What is java HashMap
description: Java Collections - HashMap
image: /assets/img/java-hashmap.jpg
optimized_image: /assets/img/java-hashmap.jpg
category: java
tags:
  - java
  - collections
author: ankurmalviya1618
---

# Understanding Java HashMap: Internal Workings and Performance

Java’s `HashMap` is one of the most widely used data structures in the Java Collections Framework. It offers fast access to key-value pairs, but its internal design can be quite complex. In this post, we will explore how a `HashMap` works under the hood, discuss key design elements like hashing, collision resolution, and load factor, and review performance improvements—especially those introduced in Java 8.

---

## 1. Overview of HashMap

A `HashMap` stores key-value pairs and allows fast retrieval, insertion, and deletion in average-case constant time, O(1). However, its performance depends heavily on the quality of the key’s `hashCode()` and `equals()` implementations.

**Key Characteristics:**

- **Unordered Collection:** The entries are not maintained in any specific order.
- **Null Support:** One null key and multiple null values are allowed.
- **Non-Synchronized:** `HashMap` is not thread-safe; for concurrent access, consider using `ConcurrentHashMap`.
- **Internal Structure:** Backed by an array of buckets where each bucket is initially a linked list (or a balanced tree under high collision scenarios in Java 8).

---

## 2. Internal Data Structure

At its core, a `HashMap` uses an array of nodes, where each node represents a bucket that holds key-value mappings. Each node is an instance of an inner static class defined roughly as follows:

```java
static class Node<K,V> implements Map.Entry<K,V> {
    final int hash;
    final K key;
    V value;
    Node<K,V> next;
    // Constructor and other methods...
}
```

The node’s hash field stores the hash value of the key, computed by a two-step process:

1. The key’s own **hashCode()** is obtained.
2. A secondary hash function is applied to further mix the bits:

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```
This processing helps distribute keys more uniformly across the buckets.

## 3. How the put() Operation Works
When you insert a key-value pair using put(), the following steps occur:

- ### Compute the Hash:
The key’s hashCode() is called, and the above hash function computes the final hash.

- ### Determine Bucket Index:
The final hash is used to calculate an index in the internal array. Typically, this is done using an operation similar to:

```java
int index = hash & (table.length - 1);
```

- ### Collision Resolution:

  - If the bucket is empty:
  The new node is simply placed in the bucket.
  - If the bucket is not empty:
  The HashMap traverses the linked list in that bucket:
    - If a node with an equal key (using equals()) is found, its value is replaced.
    - Otherwise, the new node is appended to the end of the list.
  - Treeification (Java 8+):
  If the number of nodes in a bucket exceeds a threshold (commonly 8), the linked list is converted into a balanced tree (red-black tree) to improve worst-case performance from O(n) to O(log n).

A simplified version of the put() operation looks like this:

```java
public V put(K key, V value) {
    int hash = hash(key);
    int i = indexFor(hash, table.length);
    for (Node<K,V> e = table[i]; e != null; e = e.next) {
        if (e.hash == hash && (e.key == key || (key != null && key.equals(e.key)))) {
            V oldValue = e.value;
            e.value = value;
            return oldValue;
        }
    }
    addEntry(hash, key, value, i);
    return null;
}
```

## 4. How the get() Operation Works

The get() method retrieves a value by performing similar steps:

- Hash Calculation: Compute the hash for the given key.
- Locate the Bucket: Determine the bucket index using the final hash.
- Search the Bucket: Traverse the linked list (or tree, if treeified) in the bucket:
  - Compare the stored hash and key using equals().
  - Return the corresponding value if a match is found; otherwise, return null.

A simplified get() implementation:

```java
public V get(Object key) {
    int hash = (key == null) ? 0 : hash(key);
    int i = indexFor(hash, table.length);
    for (Node<K,V> e = table[i]; e != null; e = e.next) {
        if (e.hash == hash && (e.key == key || (key != null && key.equals(e.key))))
            return e.value;
    }
    return null;
}
```

## 5. Rehashing and the Load Factor

A critical performance factor in a HashMap is its load factor, which determines when the map should resize:

- **Initial Capacity**: The number of buckets in the internal array (default is 16).
- **Load Factor**: The threshold for resizing (default is 0.75). When the number of entries exceeds capacity × loadFactor, the map is resized (typically doubled in size), and all existing entries are rehashed into the new bucket array.

### Performance Trade-offs:

- **Low Load Factor**: Fewer collisions but higher memory consumption.
- **High Load Factor**: More collisions may occur, increasing the cost of operations, but saves memory.

## 6. Performance Improvements in Java 8

Java 8 introduced several enhancements to address performance issues:

- **Treeification:** 
When many collisions occur (i.e., many keys hash to the same bucket), the linked list in that bucket is converted to a red-black tree once it reaches a threshold. This change improves worst-case lookup and update times from O(n) to O(log n).

- **Improved Hash Function:**
The rehashing algorithm better disperses the keys across the buckets, reducing the likelihood of collisions.

- **Optimized Resizing:**
The process of rehashing entries during resizing has been refined to reduce the overhead involved in expanding the table.

These improvements lead to more predictable performance, especially under heavy load or when handling large data sets.

## Conclusion
A deep understanding of HashMap’s internal workings is key to optimizing its performance in your Java applications. By knowing how keys are hashed, how collisions are resolved, and how resizing is managed, you can make informed decisions about initial capacities, load factors, and key implementations.

With the improvements in Java 8—especially treeification of buckets—HashMap now handles worst-case collision scenarios much more efficiently, ensuring robust performance even under heavy load.

> Happy Coding :)