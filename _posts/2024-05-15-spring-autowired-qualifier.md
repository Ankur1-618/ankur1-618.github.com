---
date: 2024-05-15 00:30:00 +0530
layout: post
title: Spring @Autowired & @Qualifier
subtitle: spring annotations
description: annotiations in spring
image: /assets/img/spring_framework_in_java.jpg
optimized_image: /assets/img/spring_framework_in_java.jpg
category: spring
tags:
  - code
  - java
  - spring
  - spring boot
author: ankurmalviya1618
my_prop: 'nil'
---

# Understanding Spring's @Autowired and @Qualifier Annotations

Spring’s dependency injection makes it easy to manage components and their dependencies. Two key annotations in this process are `@Autowired` and `@Qualifier`. In this blog, we'll explore how these annotations work together, especially when multiple beans of the same type exist, and provide some examples.

## What is @Autowired?

The `@Autowired` annotation allows Spring to automatically inject a bean into a property, constructor, or method. It simplifies wiring dependencies without the need for explicit XML configuration.

**Example:**

```java
@Component
public class UserService {
    
    @Autowired
    private UserRepository userRepository;
    
    // userRepository is automatically injected by Spring
}
```

In the above example, Spring finds a bean of type UserRepository and injects it into the UserService.

## Handling Multiple Beans with `@Qualifier`

Sometimes, you might have more than one bean of the same type in your application context. In such cases, Spring doesn't know which bean to inject. This is where `@Qualifier` comes in handy. It lets you specify exactly which bean should be injected by providing a unique identifier.

### Example Scenario

Imagine you have an interface `PaymentService` with two implementations: `CreditCardPaymentService` and `PaypalPaymentService`.

```java
public interface PaymentService {
    void processPayment(double amount);
}
```

Define the two implementations and annotate them with `@Component` and a qualifier:

```java
@Component
@Qualifier("creditCard")
public class CreditCardPaymentService implements PaymentService {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card payment of $" + amount);
    }
}

@Component
@Qualifier("paypal")
public class PaypalPaymentService implements PaymentService {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing PayPal payment of $" + amount);
    }
}
```

Now, in a consumer class, you can use both `@Autowired` and `@Qualifier` to specify which implementation to inject:

```java
@Component
public class PaymentProcessor {

    private PaymentService paymentService;

    // Constructor injection with qualifier
    @Autowired
    public PaymentProcessor(@Qualifier("paypal") PaymentService paymentService) {
        this.paymentService = paymentService;
    }

    public void executePayment(double amount) {
        paymentService.processPayment(amount);
    }
}
```

In this example, `PaymentProcessor` explicitly asks for the `paypal` qualified bean. You can similarly use `@Qualifier` on field or setter injections.

## Conclusion
Using `@Autowired` makes it easy to inject dependencies automatically, but when multiple beans of the same type exist, `@Qualifier` helps disambiguate which bean should be used. This combination is powerful for building flexible and maintainable Spring applications.

> Happy Coding :)