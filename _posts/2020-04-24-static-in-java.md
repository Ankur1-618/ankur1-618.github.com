---
date: 2020-04-24 15:35:00 +0530
layout: post
title: Static in Java
subtitle: In Java, static keyword is mainly used for memory management.
description: In Java, "static" is one of the keywords in Java, and it is quite complicated, let's make it a bit simple.
image: /assets/img/codingimgs/static-in-java.jpg
optimized_image: /assets/img/codingimgs/static-in-java.jpg
category: code
tags:
  - coding
  - java
author: ankurmalviya1618
---
# "Static" in Java
In Java, keywords are the reserved words that cannot be used as identifiers. In total there are 57 keywords in Java. "static" is one of the keywords in Java, and it is quite complicated, let's make it a bit simple.

## Topics
1. Introduction
2. Application 
    * Static Block
    * Static Variable
    * Static Method
    * Static Class

## Introduction
In Java, **static** keyword is mainly used for memory management. It can be used with **block**, **variables**, **methods**, and **nested classes**.

Basically, static is used for a constant variable or a method that is same for every **instance of a class**.

* The **main** method of a class is generally labelled static.

In order to create a static member (block, variable, method, nested class), you need to precede its declaration with the keyword **static**.

* When a member of the class is declared as static, it can be accessed before the objects of its class are created, and without any object reference.

## Static Block

Static block is a set of instructions that is run only once when a class is loaded in memory.<br>
The static block spans all instances of a class, i.e. the static block is same for all the instances of a class.
A class can have **multiple Static blocks**, which will execute in the same sequence in which they have been written into the program. That’s the reason, values initialized by first block are overwritten by second block.<br>
Code:


<details><summary><strong>Example :</strong></summary>
<pre style="
    background-color: bisque;
    margin-top: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    padding-top: 0px;
    padding-bottom: 0px;
">

class A {
    static {
        System.out.println("Static Block of A");
    }
}


class B extends A {
    static {
        System.out.println("Static Block of B");
    }
}


public class HelloWorld{

     public static void main(String []args){
        System.out.println("Hello World");
        A a = new A();
        System.out.println("Hello World again");
        A b = new B();
     }
}

</pre>
</details>


<details><summary><strong>Result :</strong></summary>
<pre style="
    background-color: bisque;
    margin-top: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    padding-top: 0px;
    padding-bottom: 0px;
">

Hello World
Static Block of A
Hello World again
Static Block of B

</pre>
</details>

<button type="button" class="collapsible1">Open Collapsible</button>
<div class="content1">
  <p>Lorem ipsum...</p>
</div>

## Static Variable
A static variable is common to all the instances (or objects) of the class because it is a class level variable. In other words you can say that only a single copy of static variable is created and shared among all the instances of the class. Memory allocation for such variables only happens once when the class is loaded in the memory.

* Static variables are also known as Class Variables.
* Unlike non-static variables, such variables can be accessed directly in static and non-static methods.
* Static variables are initialized
    * when class is loaded.
    * before any object of that class is created.
    * before any static method of the class executes.
    
## Static Method
Static Methods can access class variables(static variables) without using object(instance) of the class, however non-static methods and non-static variables can only be accessed using objects.<br>
Static methods can be accessed directly in static and non-static methods.

In Java, if the name of a derived class static function is the same as a base class static function then the base class static function shadows (or conceals) the derived class static function. <br>
For example, the following Java code prints “Static Method of A” for b.fun().<br>
Note: Static method is a class property, so if a static method is called from a class name or object having a class container then the method of that class is called not the object’s method.

Note : If the methods were non-static, b.fun() would print "Static method of B"<br>
Note : static method and non-static methods can't override each other.

<details><summary><strong>Code :</strong></summary>
<pre style="
    background-color: bisque;
    margin-top: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    padding-top: 0px;
    padding-bottom: 0px;
">
class A {
    static void fun() {
        System.out.println("Static Method of A");
    }
}
class B extends A {
    static void fun() {
        System.out.println("Static Method of B");
    }
}
public class HelloWorld{
     public static void main(String []args){
        System.out.println("Hello World");
        A a = new A();
        A b = new B();
        B c = new B();
        a.fun();
        b.fun();
        c.fun();
     }
}
</pre>
</details>

<details><summary><strong>Result :</strong></summary>
<pre style="
    background-color: bisque;
    margin-top: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    margin-bottom: 0px;
    padding-top: 0px;
    padding-bottom: 0px;
">
Hello World
Static Method of A
Static Method of A
Static Method of B
</pre>
</details>

## Static Class
A class can be made static only if it is a nested class. Nested static class doesn’t need a reference of Outer class. In this case, a static class cannot access non-static members of the Outer class.

* A static nested class may be instantiated without instantiating its outer class.
* Inner classes can access both static and non-static members of the outer class. A static class can access only the static members of the outer class.
