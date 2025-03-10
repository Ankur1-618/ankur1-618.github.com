---
date: 2020-04-27 15:40:00 +0530
layout: post
title: LRU Cache Implemantation in Python
subtitle: How hard could it be to implement a LRU cache in python? Let's find out.
description: How hard could it be to implement a LRU cache in python? Let's find out.
image: /assets/img/codingimgs/LRU-cache-implementation.jpg
optimized_image: /assets/img/codingimgs/LRU-cache-implementation.jpg
category: code
tags:
  - coding
  - python
  - algorithms
author: ankurmalviya1618
---

# LRU Cache
The [LRU caching](https://en.wikipedia.org/wiki/Cache_replacement_policies#LRU) scheme is to remove the least recently used frame when the cache is full and a new page is referenced which is not there in cache.

## Question

Design and implement a data structure for **Least Recently Used (LRU) cache**. It should support the following operations: *get* and *put*.

**get(key)** - Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.<br>
**set(key, value)** - Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

The cache is initialized with a positive capacity.

> Could you do both operations in O(1) time complexity?

***
# LRU Cache implementation in Python
First of all let's see what do we need to implement LRU cache in python
- A reasonable high performance hash table
- The bookkeeping to track the access.

The below example is naive implementation:
```python
class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.tm = 0
        self.cache = {}
        self.lru = {}

    def get(self, key):
        if key in self.cache:
            self.lru[key] = self.tm
            self.tm += 1
            return self.cache[key]
        return -1

    def set(self, key, value):
        if len(self.cache) >= self.capacity:
            # find the LRU entry
            old_key = min(self.lru.keys(), key=lambda k:self.lru[k])
            self.cache.pop(old_key)
            self.lru.pop(old_key)
        self.cache[key] = value
        self.lru[key] = self.tm
        self.tm += 1
```
We use cache to store the (key, value) mapping, and lru and automatic incremented tm to track the access history, pretty straightforward, right?
It turns out this implementation performs poorly in a more realistic test case, and here is the profiling result:

```python
python -m cProfile lru-cache-test.py naive-lru-cache
... ...
lrucache took 1.478 sec
     4180120 function calls in 1.500 seconds

Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.024    0.024    1.500    1.500 lru-cache-test.py:1(<module>)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:10(__exit__)
    12558    0.006    0.000    1.458    0.000 lru-cache-test.py:25(set)
     7941    0.003    0.000    0.007    0.000 lru-cache-test.py:28(get)
    20499    0.002    0.000    0.002    0.000 lru-cache-test.py:32(<lambda>)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:5(Timer)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:6(__enter__)
     7941    0.004    0.000    0.004    0.000 naive-lru-cache.py:15(get)
    12558    0.022    0.000    1.452    0.000 naive-lru-cache.py:22(set)
  4098048    0.606    0.000    0.606    0.000 naive-lru-cache.py:25(<lambda>)
        1    0.000    0.000    0.000    0.000 naive-lru-cache.py:6(<module>)
        1    0.000    0.000    0.000    0.000 naive-lru-cache.py:8(LRUCache)
        1    0.000    0.000    0.000    0.000 naive-lru-cache.py:9(__init__)
        1    0.007    0.007    0.007    0.007 {__import__}
        1    0.003    0.003    0.005    0.005 {filter}
    12559    0.001    0.000    0.001    0.000 {len}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
     2001    0.025    0.000    0.025    0.000 {method 'keys' of 'dict' objects}
     4002    0.001    0.000    0.001    0.000 {method 'pop' of 'dict' objects}
     2001    0.797    0.000    1.403    0.001 {min}
        2    0.000    0.000    0.000    0.000 {time.clock}
```

It shows that the significant CPU time, 1.403 out of 1.478 is spent on the min operation, more concretely, this statement:
```python
old_key = min(self.lru.keys(), key=lambda k:self.lru[k])
```
We naively identify the least-recently-used item by a linear search with time complexity O(n)O(n) instead of O(1)O(1), a clear violation of the set’s requirement.

In the contrast of the traditional hash table, the get and set operations are both write operation in LRU cache. The timestamp is mere the order of the operation. So an ordered hash table, aka OrderedDict, might be able to meet our needs. Here is the LRU cache implementation based on OrderedDict:
```python
import collections

class LRUCache:
    def __init__(self, capacity):
        self.capacity = capacity
        self.cache = collections.OrderedDict()

    def get(self, key):
        try:
            value = self.cache.pop(key)
            self.cache[key] = value
            return value
        except KeyError:
            return -1

    def set(self, key, value):
        try:
            self.cache.pop(key)
        except KeyError:
            if len(self.cache) >= self.capacity:
                self.cache.popitem(last=False)
        self.cache[key] = value
```

The implementation is much cleaner as all the order bookkeeping is handled by the OrderDict now. For each get and set operation, we first pop the item, then insert back to update its timestamp. The element in the head of sequence is the least-used-item, thus the candidate to expire if the maximum capacity is reached. Here is the profiling result for the sake of comparison:

```python
python -m cProfile lru-cache-test.py ordered-lru-cache
... ...
lrucache took 1.478 sec
lrucache took 0.122 sec
     149493 function calls (149490 primitive calls) in 0.138 seconds

Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.000    0.000 _abcoll.py:526(update)
        3    0.000    0.000    0.000    0.000 _abcoll.py:98(__subclasshook__)
        3    0.000    0.000    0.000    0.000 _weakrefset.py:16(__init__)
        3    0.000    0.000    0.000    0.000 _weakrefset.py:20(__enter__)
        3    0.000    0.000    0.000    0.000 _weakrefset.py:26(__exit__)
        2    0.000    0.000    0.000    0.000 _weakrefset.py:36(__init__)
        3    0.000    0.000    0.000    0.000 _weakrefset.py:52(_commit_removals)
        6    0.000    0.000    0.000    0.000 _weakrefset.py:58(__iter__)
        5    0.000    0.000    0.000    0.000 _weakrefset.py:68(__contains__)
        3    0.000    0.000    0.000    0.000 _weakrefset.py:81(add)
        1    0.000    0.000    0.000    0.000 abc.py:128(__instancecheck__)
      3/2    0.000    0.000    0.000    0.000 abc.py:148(__subclasscheck__)
        1    0.001    0.001    0.002    0.002 collections.py:1(<module>)
    22494    0.020    0.000    0.038    0.000 collections.py:132(pop)
     1995    0.003    0.000    0.010    0.000 collections.py:153(popitem)
        1    0.000    0.000    0.000    0.000 collections.py:26(OrderedDict)
        1    0.000    0.000    0.000    0.000 collections.py:26(OrderedDict)
        1    0.000    0.000    0.000    0.000 collections.py:38(__init__)
        1    0.000    0.000    0.000    0.000 collections.py:381(Counter)
    18500    0.021    0.000    0.021    0.000 collections.py:54(__setitem__)
    16452    0.016    0.000    0.018    0.000 collections.py:64(__delitem__)
     3990    0.001    0.000    0.001    0.000 collections.py:73(__iter__)
        1    0.001    0.001    0.001    0.001 heapq.py:31(<module>)
        1    0.000    0.000    0.000    0.000 keyword.py:11(<module>)
        1    0.024    0.024    0.138    0.138 lru-cache-test.py:1(<module>)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:10(__exit__)
    12558    0.009    0.000    0.073    0.000 lru-cache-test.py:25(set)
     7941    0.005    0.000    0.034    0.000 lru-cache-test.py:28(get)
    20499    0.002    0.000    0.002    0.000 lru-cache-test.py:32(<lambda>)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:5(Timer)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:6(__enter__)
        1    0.000    0.000    0.000    0.000 ordered-lru-cache.py:11(LRUCache)
        1    0.000    0.000    0.000    0.000 ordered-lru-cache.py:12(__init__)
     7941    0.009    0.000    0.029    0.000 ordered-lru-cache.py:16(get)
    12558    0.019    0.000    0.064    0.000 ordered-lru-cache.py:24(set)
        1    0.000    0.000    0.002    0.002 ordered-lru-cache.py:6(<module>)
     7941    0.005    0.000    0.034    0.000 lru-cache-test.py:28(get)
    20499    0.002    0.000    0.002    0.000 lru-cache-test.py:32(<lambda>)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:5(Timer)
        1    0.000    0.000    0.000    0.000 lru-cache-test.py:6(__enter__)
        1    0.000    0.000    0.000    0.000 ordered-lru-cache.py:11(LRUCache)
        1    0.000    0.000    0.000    0.000 ordered-lru-cache.py:12(__init__)
     7941    0.009    0.000    0.029    0.000 ordered-lru-cache.py:16(get)
    12558    0.019    0.000    0.064    0.000 ordered-lru-cache.py:24(set)
        1    0.000    0.000    0.002    0.002 ordered-lru-cache.py:6(<module>)
        1    0.001    0.001    0.003    0.003 {__import__}
        1    0.003    0.003    0.005    0.005 {filter}
        4    0.000    0.000    0.000    0.000 {getattr}
        1    0.000    0.000    0.000    0.000 {hasattr}
        1    0.000    0.000    0.000    0.000 {isinstance}
      4/2    0.000    0.000    0.000    0.000 {issubclass}
     1995    0.001    0.000    0.001    0.000 {iter}
     4047    0.000    0.000    0.000    0.000 {len}
        2    0.000    0.000    0.000    0.000 {method '__subclasses__' of 'type' objects}
        6    0.000    0.000    0.000    0.000 {method 'add' of 'set' objects}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
        1    0.000    0.000    0.000    0.000 {method 'items' of 'dict' objects}
    16452    0.002    0.000    0.002    0.000 {method 'pop' of 'dict' objects}
        3    0.000    0.000    0.000    0.000 {method 'remove' of 'set' objects}
     1995    0.000    0.000    0.001    0.000 {next}
        2    0.000    0.000    0.000    0.000 {time.clock}
```
