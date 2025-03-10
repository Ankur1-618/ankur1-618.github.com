---
date: 2023-05-17 15:40:00 +0530
layout: post
title: Throwables in Java
subtitle: Let's find out the secret behind Errors, Exceptions and Throwables in Java and its Hierarchy.
description: let's find out the secret behind Errors, Exceptions and Throwables in Java and its Hierarchy.instances whose state doesn’t change after it has been initialized.
image: /assets/img/codingimgs/Throwables-in-java.jpg
optimized_image: /assets/img/codingimgs/Throwables-in-java.jpg
category: code
tags:
  - throwable
  - coding
  - java
author: ankurmalviya1618
---

# Throwables in Java
The Throwable class is the superclass of all errors and exceptions in the Java language. Only objects that are instances of this class (or of one of its subclasses) are thrown by the Java Virtual Machine or can be thrown by the Java throw statement. Similarly, only this class or one of its subclasses can be the argument type in a catch clause.

A Throwable class contains a snapshot of the execution stack of its thread at the time it was created. It can also contain a message string that gives more information about the error.

Following is the declaration for [**java.lang.Throwable**](https://docs.oracle.com/javase/7/docs/api/java/lang/Throwable.html) class −
```java
public class Throwable
   extends Object
      implements Serializable
```
<br><br>
This class inherits methods from [**java.lang.Object**](https://docs.oracle.com/javase/7/docs/api/java/lang/Object.html) ::
- protected Object clone()
- boolean equals(Object obj)
- protected void finalize()
- Class<?> getClass()
- int hashCode()
- void notify()
- void notifyAll()
- String toString()
- void wait()
- void wait(long timeout)
- void wait(long timeout, int nanos)
<br><br>

Let's see some **Constructors** for Throwable class::


<table>
    <thead>
      <tr>
        <th>Sr. No.</th>
        <th>Constructor</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
        <tr>
            <td>1. </td>
            <td>Throwable()</td>
            <td>This constructs a new throwable with null as its detail message.</td>
        </tr>
        <tr>
            <td>2. </td>
            <td>Throwable(String message)</td>
            <td>This constructs a new throwable with the specified detail message.</td>
        </tr>
        <tr>
            <td>3. </td>
            <td>Throwable(String message, Throwable cause)</td>
            <td>This constructs a new throwable with the specified detail message and cause.</td>
        </tr>
        <tr>
            <td>4. </td>
            <td>Throwable(Throwable cause)</td>
            <td>This constructs a new throwable with the specified cause and a detail message of (cause==null ? null : cause.toString()) (which typically contains the class and detail message of cause).</td>
        </tr>
    </tbody>
  </table>

<br><br>
And now let's explore the class methods ::

<table>
    <thead>
        <tr>
          <th>Sr. No.</th>
          <th>Constructor</th>
          <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1. </td>
            <td>Throwable fillInStackTrace()</td>
            <td>This method fills in the execution stack trace.</td>
        </tr>
        <tr>
            <td>2. </td>
            <td>Throwable getCause()</td>
            <td>This method returns the cause of this throwable or null if the cause is nonexistent or unknown.</td>
        </tr>
        <tr>
            <td>3. </td>
            <td>String getLocalizedMessage()</td>
            <td>This method creates a localized description of this throwable. Subclasses may override this method in order to produce a locale-specific message. For subclasses that do not override this method, the default implementation returns the same result as getMessage().</td>
        </tr>
        <tr>
            <td>4. </td>
            <td>String getMessage()</td>
            <td>This method returns the detail message string of this throwable.</td>
        </tr>
        <tr>
            <td>5. </td>
            <td>StackTraceElement[] getStackTrace()</td>
            <td>This method provides programmatic access to the stack trace information printed by printStackTrace().</td>
        </tr>
        <tr>
            <td>6. </td>
            <td>Throwable initCause(Throwable cause)</td>
            <td>This method initializes the cause of this throwable to the specified value.</td>
        </tr>
        <tr>
            <td>7. </td>
            <td>void printStackTrace()</td>
            <td>This method prints this throwable and its backtrace to the standard error stream.</td>
        </tr>
        <tr>
            <td>8. </td>
            <td>void printStackTrace(PrintStream s)</td>
            <td>This method prints this throwable and its backtrace to the specified print stream.</td>
        </tr>
        <tr>
            <td>9. </td>
            <td>void printStackTrace(PrintWriter s)</td>
            <td>This method prints this throwable and its backtrace to the specified print writer.</td>
        </tr>
        <tr>
            <td>10. </td>
            <td>void setStackTrace(StackTraceElement[] stackTrace)</td>
            <td>This method sets the stack trace elements that will be returned by getStackTrace() and printed by printStackTrace() and related methods.</td>
        </tr>
        <tr>
            <td>11. </td>
            <td>String toString()</td>
            <td>This method returns a short description of this throwable.</td>
        </tr>
    </tbody>
</table>

<br><br>
Okay, that was too much of methods and constructors, but this is the basic which is common to all *Throwables*. Further moving on, we will see the heirarchy of Throwable class but before that some basics on Throwable::

A *Throwable* instance contains the current execution stack, captured when the error exception occurred. It can also contain a message (obtained via the *getMessage()* method), indicating the relevant error message. Lastly, for exception chains where one error causes another error to be thrown, a *Throwable* can obtain a potential cause (collected via the *getCause()* method) as well.

Let's dive into the hierarchy now ::
Throwable class is extended by two classes :
- Exception
- Error

<br><br>
Let's see the hierarchy ::
- Throwable
    - Error
        - AssertionError
        - LinkageError
            - BootstrapMethodError
            - ClassCircularityError
            - ClssFormatError
                - UnsupportedClassVersionError
            - ExceptionInInitializerError
            - IncompatibleClassChangeError
                - AbstractMethodError
                - IllegalAccessError
                - InstantiationError
                - NoSuchFieldError
                - NoSuchMethodError
            - NoClassDefFoundError
            - UnsatisfiedLinkError
            - VerifyError
        - ThreadDeath
        - VirtualMachineError
            - InternalError
            - OutOfMemoryError
            - StackOverflowError
            - UnknownError
    - Exception
        - CloneNotSupportedException
        - InterruptedException
        - IOException
            - FileNotFoundException
            - SocketException
                - ConnectException
            - UnknownHostException
        - ReflectiveOperationException
            - ClassNotFoundException
            - IllegalAccessException
            - InstantiationException
            - InvocationTargetException
            - NoSuchFieldException
            - NoSuchMethodException
        - RuntimeException
            - ArithmeticException
            - ArrayStoreException
            - ClassCastException
            - ConcurrentModificationException
            - EnumConstantNotPresentException
            - IllegalArgumentException
                - IllegalThreadStateException
                - NumberFormatException
            - IllegalMonitorStateException
            - IllegalStateException
            - IndexOutOfBoundsException
                - ArrayIndexOutOfBoundsException
                - StringIndexOutOfBoundsException
            - NegativeArraySizeException
            - NullPointerException
            - SecurityException
            - TypeNotPresentException
            - UnsupportedOperationException
<br><br>

I wish I could show the hierarchy more visibly but this is the most creative idea that came to my mind. Lets see it in visual format.

<img src="{{site.baseurl}}/assets/img/codingimgs/Throwable_Error_Hierarchy.gif" alt="Error Hierarchy"
	title="Error Hierarchy" width="100%" height="100%" />

<img src="{{site.baseurl}}/assets/img/codingimgs/Throwable_Exception_Hierarchy.gif" alt="Exception Hierarchy"
	title="Exception Hierarchy" width="100%" height="100%" />

