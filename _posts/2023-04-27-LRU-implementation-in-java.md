---
date: 2023-04-27 15:40:00 +0530
layout: post
title: LRU Cache Implemantation in Java
subtitle: How hard could it be to implement a LRU cache in java? Let's find out.
description: How hard could it be to implement a LRU cache in java? Let's find out.
image: /assets/img/codingimgs/LRU-cache-implementation-java.jpg
optimized_image: /assets/img/codingimgs/LRU-cache-implementation-java.jpg
category: code
tags:
  - coding
  - java
  - algorithms
author: ankurmalviya1618
---

# LRU Cache
The [LRU caching](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU) scheme is to remove the least recently used frame when the cache is full and a new page is referenced which is not there in cache.

# Implementing an LRU Cache in Java

In this post, we'll explore how to implement a Least Recently Used (LRU) Cache in Java. An LRU Cache is a data structure that stores a limited number of items and removes the least recently accessed item when the cache reaches its capacity.

## What is an LRU Cache?

An LRU Cache evicts the least recently used item when it reaches its capacity limit. This eviction policy ensures that frequently accessed items remain in the cache, improving performance for repeated accesses.

<img src="/assets/img/codingimgs/LRU1.gif" alt="LRU Image">

## Key Operations

An LRU Cache supports the following operations:

- `get(key)`: Retrieve the value associated with the key if it exists in the cache; otherwise, return `-1`.
- `put(key, value)`: Insert or update the key-value pair in the cache. If the cache exceeds its capacity, evict the least recently used item.

## Implementation Details

To implement an LRU Cache, we need a data structure that allows for efficient insertion, deletion, and access operations. A combination of a **HashMap** and a **Doubly Linked List** is commonly used:

- **HashMap**: Provides O(1) time complexity for `get` and `put` operations.
- **Doubly Linked List**: Maintains the order of access, allowing us to quickly move elements to the front (most recently used) or remove the last element (least recently used).

<hr>
<img src="/assets/img/codingimgs/LRU2.gif" alt="LRU Image"><br>
<img src="/assets/img/codingimgs/LRU3.gif" alt="LRU Image">
<hr>

## Java Implementation

Below is a Java implementation of an LRU Cache:

```java
import java.util.*;

class LRUCache {

    private final int capacity;
    private final Map<Integer, Node> cache;
    private final DoublyLinkedList dll;

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.cache = new HashMap<>();
        this.dll = new DoublyLinkedList();
    }

    public int get(int key) {
        if (!cache.containsKey(key)) {
            return -1;
        }
        Node node = cache.get(key);
        dll.moveToFront(node);
        return node.value;
    }

    public void put(int key, int value) {
        if (cache.containsKey(key)) {
            Node node = cache.get(key);
            node.value = value;
            dll.moveToFront(node);
        } else {
            if (cache.size() >= capacity) {
                Node lru = dll.removeLast();
                cache.remove(lru.key);
            }
            Node newNode = new Node(key, value);
            dll.addFirst(newNode);
            cache.put(key, newNode);
        }
    }

    private static class Node {
        int key;
        int value;
        Node prev;
        Node next;

        Node(int key, int value) {
            this.key = key;
            this.value = value;
        }
    }

    private static class DoublyLinkedList {
        private final Node head;
        private final Node tail;

        DoublyLinkedList() {
            head = new Node(0, 0);
            tail = new Node(0, 0);
            head.next = tail;
            tail.prev = head;
        }

        void addFirst(Node node) {
            node.next = head.next;
            node.prev = head;
            head.next.prev = node;
            head.next = node;
        }

        void moveToFront(Node node) {
            remove(node);
            addFirst(node);
        }

        void remove(Node node) {
            node.prev.next = node.next;
            node.next.prev = node.prev;
        }

        Node removeLast() {
            if (tail.prev == head) {
                return null;
            }
            Node last = tail.prev;
            remove(last);
            return last;
        }
    }
}
```

## Usage Example

```java
public class Main {
    public static void main(String[] args) {
        LRUCache cache = new LRUCache(2);
        cache.put(1, 1);
        cache.put(2, 2);
        System.out.println(cache.get(1)); // returns 1
        cache.put(3, 3); // evicts key 2
        System.out.println(cache.get(2)); // returns -1 (not found)
        cache.put(4, 4); // evicts key 1
        System.out.println(cache.get(1)); // returns -1 (not found)
        System.out.println(cache.get(3)); // returns 3
        System.out.println(cache.get(4)); // returns 4
    }
}
```

## Explanation
- Node Class: Represents each entry in the cache with key, value, and pointers to the previous and next nodes.
- DoublyLinkedList Class: Manages the order of nodes, with methods to add a node to the front, remove a node, and remove the last node.
- LRUCache Class: Contains the cache capacity, a HashMap for quick access, and a DoublyLinkedList to track the usage order.

## Conclusion
Implementing an LRU Cache in Java requires a good understanding of data structures to ensure efficient operations. By combining a HashMap and a DoublyLinkedList, we achieve constant time complexity for both get and put operations, ensuring optimal performance.
