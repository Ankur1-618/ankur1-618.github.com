---
date: 2023-04-23 14:30:00 +0530
layout: post
title: Immutable Class in Java
subtitle: Immutable objects are instances whose state doesn’t change after it has been initialized.
description: Today we will learn how to create an immutable class in Java. Immutable objects are instances whose state doesn’t change after it has been initialized.
image: /assets/img/codingimgs/immutable-class-in-java1.jpg
optimized_image: /assets/img/codingimgs/immutable-class-in-java1.jpg
category: code
tags:
  - coding
  - java
author: ankurmalviya1618
---
# Immutable Class in Java

## Introduction
In this post, we'll explore how to create an **immutable class** in Java. Immutable objects are instances whose state doesn’t change once they’re initialized. A classic example is the `String` class, and all wrapper classes in `java.lang`—like `Integer`, `Boolean`, `Character`, `Byte`, `Short`, `Long`, `Float`, `Double`, `BigDecimal`, and `BigInteger`—are immutable as well.

## Properties
- **Caching**: Immutable objects are ideal for caching because their values never change.
- **Thread-Safety**: They’re inherently thread-safe, removing the need for synchronization in multi-threaded environments.
- **Shared References**: References to immutable objects can be easily shared or cached without concerns about value modifications.
- **Map/Set Usage**: Immutable objects can serve as map keys or set values since they won’t change after creation.

## How to Make an Immutable Class
There aren’t strict language-enforced rules for immutability, but the goal is to prevent modifications to a class’s fields after construction. Consider the following steps:

1. **Mark the Class as `final`**  
   Prevent extension by declaring the class as `final`.
2. **Use Private Fields**  
   Keep fields `private` to restrict direct access.
3. **No Setters**  
   Omit setter methods to disallow changes to field values.
4. **Use `final` Fields**  
   Mark mutable fields as `final` so their values can only be assigned once.
5. **Initialize via Constructor (Deep Copy)**  
   Assign values to all fields in the constructor. If a field is a mutable object, perform a deep copy to avoid referencing external mutable objects.
6. **Clone Objects in Getters**  
   When returning mutable objects, return a copy (clone) instead of the original reference.

By following these guidelines, you can ensure that your Java class remains immutable.

```java
import java.util.HashMap;
import java.util.Iterator;

public final class FinalClassExample {

	private final int id;
	private final String name;
	private final HashMap<String,String> testMap;

	public int getId() {
		return id;
	}

	public String getName() {
		return name;
	}

	public HashMap<String, String> getTestMap() {
		//return testMap;
		return (HashMap<String, String>) testMap.clone();
	}

	public FinalClassExample(int i, String n, HashMap<String,String> hm){
		System.out.println("Performing Deep  for Object initialization");
		this.id=i;
		this.name=n;
		HashMap<String,String> tempMap=new HashMap<String,String>();
		String key;
		Iterator<String> it = hm.keySet().iterator();
		while(it.hasNext()){
			key=it.next();
			tempMap.put(key, hm.get(key));
		}
		this.testMap=tempMap;
	}

	public static void main(String[] args) {
		HashMap<String, String> h1 = new HashMap<String,String>();
		h1.put("1", "first");
		h1.put("2", "second");		
		String s = "original";		
		int i=10;		
		FinalClassExample ce = new FinalClassExample(i,s,h1);
		System.out.println(s==ce.getName());
		System.out.println(h1 == ce.getTestMap());
		System.out.println("ce id:"+ce.getId());
		System.out.println("ce name:"+ce.getName());
		System.out.println("ce testMap:"+ce.getTestMap());
		i=20;
		s="modified";
		h1.put("3", "third");
		System.out.println("ce id after local variable change:"+ce.getId());
		System.out.println("ce name after local variable change:"+ce.getName());
		System.out.println("ce testMap after local variable change:"+ce.getTestMap());
		
		HashMap<String, String> hmTest = ce.getTestMap();
		hmTest.put("4", "new");
		System.out.println("ce testMap after changing variable from accessor methods:"+ce.getTestMap());

	}

}
```
```java
Performing Deep  for Object initialization
true
false
ce id:10
ce name:original
ce testMap:{2=second, 1=first}
ce id after local variable change:10
ce name after local variable change:original
ce testMap after local variable change:{2=second, 1=first}
ce testMap after changing variable from accessor methods:{2=second, 1=first}
```

## Miscellaneous
Immutable classes can also be created using builder pattern. Builder Pattern is a better option if the immutable class has a lot of attributes and some of them are optional.\
Using builder pattern to create immutable class is a good approach when the number of arguments in the Constructor is more that can cause confusion in their ordering.

**Steps to follow**
1. The immutable class should have only getter methods.
2. The immutable class will have a private constructor with Builder object as parameter that will be used to create the immutable class.
3. If the immutable class attributes are not immutable, for example HashMap, we should perform deep copy or cloning to avoid modification of its attributes.


```java
import java.util.HashMap;

public class ImmutableClass {
	
	//required fields
	private int id;
	private String name;
	
	//optional fields
	private HashMap<String, String> properties;
	private String company;
	
	public int getId() {
		return id;
	}

	public String getName() {
		return name;
	}

	public HashMap<String, String> getProperties() {
		//return cloned object to avoid changing it by the client application
		return (HashMap<String, String>) properties.clone();
	}

	public String getCompany() {
		return company;
	}

	private ImmutableClass(ImmutableClassBuilder builder) {
		this.id = builder.id;
		this.name = builder.name;
		this.properties = builder.properties;
		this.company = builder.company;
	}
	
        //Builder class
	public static class ImmutableClassBuilder{
		//required fields
		private int id;
		private String name;
		
		//optional fields
		private HashMap<String, String> properties;
		private String company;
		
		public ImmutableClassBuilder(int i, String nm){
			this.id=i;
			this.name=nm;
		}
		
		public ImmutableClassBuilder setProperties(HashMap<String,String> hm){
			this.properties = (HashMap<String, String>) hm.clone();
			return this;
		}
		
		public ImmutableClassBuilder setCompany(String comp){
			this.company = comp;
			return this;
		}
		
		public ImmutableClass build(){
			return new ImmutableClass(this);
		}
	}
}
```

**Reference** :: [Oracle Documentation](https://docs.oracle.com/javase/tutorial/essential/concurrency/imstrat.html)