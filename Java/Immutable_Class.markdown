# Immutable Class
1. Introduction
2. Properties
3. How to make an immutable class
4. Miscellaneous

## Introduction
Today we will learn how to create an immutable class in Java. Immutable objects are instances whose state doesn’t change after it has been initialized.

String is a good example for Immutable Class in Java. All wrapper classes in java.lang are immutable – String, Integer, Boolean, Character, Byte, Short, Long, Float, Double, BigDecimal, BigInteger

## Properties
* An immutable class is good for caching purpose because you don’t need to worry about the value changes. 
* Immutable class is inherently thread-safe, so you don’t need to worry about thread safety in case of multi-threaded environment.
* The references to the immutable objects can be easily shared or cached without having to copy or clone them as there state can not be changed ever after construction.
* The Immutable Class can be used as Keys of a **map** and values of  a **Set**

## How to make an immutable class
There are no specific rules to create immutable objects, the idea is to restrict the access of the fields of a class after initialization.

To create a Immutable Class, we can follow the following steps :-
1. Declare the class as **final** so it can’t be extended.
2. Make all fields **private** so that direct access is not allowed.
3. Don’t provide setter methods for variables
4. Make all mutable fields **final** so that it’s value can be assigned only once.
5. Initialize all the fields via a constructor performing **deep copy**.
6. Perform cloning of objects in the getter methods to return a copy rather than returning the actual object reference.

**Example** :
```
package com.journaldev.java;

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

	/**
	 * Accessor function for mutable objects
	 */
	public HashMap<String, String> getTestMap() {
		//return testMap;
		return (HashMap<String, String>) testMap.clone();
	}

	/**
	 * Constructor performing Deep 
	 * @param i
	 * @param n
	 * @param hm
	 */
	
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
	
	
	/**
	 * Constructor performing Shallow 
	 * @param i
	 * @param n
	 * @param hm
	 */
	/**
	public FinalClassExample(int i, String n, HashMap<String,String> hm){
		System.out.println("Performing Shallow  for Object initialization");
		this.id=i;
		this.name=n;
		this.testMap=hm;
	}
	*/
	
	/**
	 * To test the consequences of Shallow  and how to avoid it with Deep  for creating immutable classes
	 * @param args
	 */
	public static void main(String[] args) {
		HashMap<String, String> h1 = new HashMap<String,String>();
		h1.put("1", "first");
		h1.put("2", "second");
		
		String s = "original";
		
		int i=10;
		
		FinalClassExample ce = new FinalClassExample(i,s,h1);
		
		//Lets see whether its copy by field or reference
		System.out.println(s==ce.getName());
		System.out.println(h1 == ce.getTestMap());
		//print the ce values
		System.out.println("ce id:"+ce.getId());
		System.out.println("ce name:"+ce.getName());
		System.out.println("ce testMap:"+ce.getTestMap());
		//change the local variable values
		i=20;
		s="modified";
		h1.put("3", "third");
		//print the values again
		System.out.println("ce id after local variable change:"+ce.getId());
		System.out.println("ce name after local variable change:"+ce.getName());
		System.out.println("ce testMap after local variable change:"+ce.getTestMap());
		
		HashMap<String, String> hmTest = ce.getTestMap();
		hmTest.put("4", "new");
		
		System.out.println("ce testMap after changing variable from accessor methods:"+ce.getTestMap());

	}

}

```

**Output** :
```
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

**Example**
```
package com.journaldev.design.builder;

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