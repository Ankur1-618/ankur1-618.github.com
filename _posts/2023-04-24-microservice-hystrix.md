---
date: 2023-04-24 15:35:00 +0530
layout: post
title: Netflix Hystrix 
subtitle: Circuit Breaker Design Pattern Using Netflix Hystrix
description: Implementing a Basic Circuit Breaker with Hystrix in Spring Boot Microservices
image: /assets/img/hystrix.jpg
optimized_image: /assets/img/hystrix.jpg
category: code
tags:
  - spring
  - microservice
  - java
author: ankurmalviya1618
---
# Implementing Circuit Breaking with Hystrix: Enhancing Microservices Resilience
In this article, we'll build two applications: one simple microservice that returns a list of strings, and another that consumes this service. We will then integrate a Hystrix command to introduce circuit-breaking capabilities, demonstrating how to gracefully handle service failures in a microservices architecture.

---

## Introduction
A circuit breaker is an electrical component that intentionally opens a circuit to stop current flow, protecting against damage from overloads or short circuits. While fuses also serve this purpose, they operate only once and must be replaced, whereas a circuit breaker can be reset to resume normal operation.

In a microservices architecture, the circuit breaker pattern plays a similar role. When multiple services depend on each other, a failure or exception in one service can cascade upstream, eventually affecting the end-user. By implementing a circuit breaker, you can prevent such cascading failures and provide a fallback or default behavior when a service fails.

`Netflix Hystrix` is a popular library that implements this pattern. It introduces fault and latency tolerance by isolating failures, preventing them from affecting other parts of the system, and ultimately building a more robust distributed application. Additionally, Hystrix offers a user-friendly dashboard to monitor the health and status of services, which is particularly useful as the number of microservices grows and managing their state becomes more challenging.

Overall, the circuit breaker pattern—and tools like Hystrix—help maintain application stability and resilience in complex microservices environments.

![Micro1](/assets/img/micro1.png)

## Overview
1. Create a Hystrix application for circuit breaking

2. Configure the Hystrix application.

3. Hystrix Dashboards for Monitoring service state and health.

4. Learn to recover from failover issue.

A **circuit breaker** in microservices mimics its electrical counterpart. When operating normally (closed state), requests follow the standard route. However, when a fault occurs (open state), the circuit breaker trips and routes traffic to a fallback service. This prevents cascading failures throughout the system.

Netflix popularized this pattern with an example where a tailored movie recommendation service falls back to a simpler service returning the top 10 family-friendly movies when errors occur. This safe failover mechanism is a classic use case for circuit breaking.

---

## How Circuit Breaking Works

- **Normal Operation (Closed State):**  
  Traffic flows to the primary service, and everything functions as expected.

- **Fault Condition (Open State):**  
  If the primary service fails or latency exceeds set thresholds, the circuit breaker opens. Traffic is then rerouted to a backup or default service, ensuring continuous operation.

- **Monitoring and Metrics:**  
  Hystrix provides configuration properties to set thresholds for failures and latency. These properties determine when the circuit breaker should trip, allowing you to fine-tune behavior for long-running commands.

---

## Integrating Hystrix in Your Application

### Step 1: Create Two Microservices

1. **Primary Service:**  
   A basic microservice that returns a list of strings.

2. **Consumer Service:**  
   A microservice that calls the primary service.

### Step 2: Implement a Hystrix Command

The Hystrix command wraps calls to the primary service, providing circuit breaking. When a failure is detected, it directs the request to an alternative path or a fallback method.

**Key Concepts:**

- **Closed State:**  
  Traffic is processed normally.

- **Open State:**  
  When errors or high latency are detected, Hystrix reroutes requests to a fallback method, ensuring that the system remains responsive.

- **Properties and Configuration:**  
  Hystrix allows you to configure thresholds for errors and timeouts, helping you define when the circuit should open.

---

## Hystrix Application
Create a Spring Boot Application named `simple-product-service-application` using STS 4 and add web dependency to the project. Now edit the main class as follows.

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;

@RestController
@SpringBootApplication
public class SimpleProductServiceApplication {

  @GetMapping(value = "/products")
  public List<String> getAllproduct(){
    return Arrays.asList(“[“Eureka”,“Hystrix”,“Zuul”,“Ribbon””)
  }

  public static void main(String[] args) {
    SpringApplication.run(SimpleProductServiceApplication.class, args);
  }
}
```

Next set `server.port=8090`  in `application.properties` and run the application as Spring Boot Application and hit the URL as `http://localhost:8090/products`, you will see the response [“Eureka”, “Hystrix”, “Zuul”, “Ribbon”] in the browser.

Next, create another spring boot application `simple-client-application` with web and Hystrix dependency on classpath that will act as client microservice as follows.

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.client.RestTemplate;
import java.net.URI;

@RestController
@SpringBootApplication
public class SimpleClientApplication {

  @GetMapping
  public List<String> cloudProductList() {

RestTemplate restTemplate = new RestTemplate();
    URI uri = URI.create("http://localhost:8090/products");

    return restTemplate.getForObject(uri, List.class);
  }

  public static void main(String[] args) {
    SpringApplication.run(SimpleClientApplication.class, args);
  }
}
```
Add `server.port = 8080` in application.properties.

Next in browser type the URL `http://localhost:8080` and see a list of cloud components as [“Eureka”, “Hystrix”, “Zuul”, “Ribbon”] in the browser.

`RestTemplate` is used to consume other spring rest services. Its `getForObject()` will call the SimpleServiceApplication `/products` endpoint and return the List of String.

---

## Circuit Breaking With Hystrix

Now, what will happen if SimpleProductService fails or down? Stop the SimpleProductService and hit the browser with `http://localhost:8080` and you will see an error page.

![Micro2](/assets/img/micro2.png)

That’s where the Hystrix comes in. It provides us a feature that will allow the traffic to the other route so that at least end-user do not need to see this type of error page. This type of exception propagates through the application in a distributed environment and the worst case makes the whole application broken down.

In that case, you can display a tailored list of the product either based on user preference or simply a static list. To do that you need two annotations.

At the top of the class add `@EnableCircuitBreaker`. It makes this class be able to redirect the traffic in case of any failure. Another annotation you need is `@HystrixCommand`. It takes a parameter `fallbackMethod` which is the name of the method to which the request will be redirected. The resulting code will be the following.

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.client.RestTemplate;
import java.net.URI;

@RestController
@SpringBootApplication
@EnableCircuitBreaker
public class SimpleClientApplication {

  @GetMapping
  @HystrixCommand(fallbackMethod = "defaultProductList")
  public String cloudProductList() {
    RestTemplate restTemplate = new RestTemplate();
    URI uri = URI.create("http://localhost:8090/products");

    return restTemplate.getForObject(uri, String.class);
  }
public List<String> defaultProductList() {
    return Arrays.asList("spring cloud");
  }


  public static void main(String[] args) {
    SpringApplication.run(SimpleClientApplication.class, args);
  }
}
```

Now if the `SimpleProductService` is down then request will be redirected to `defaultProductList` method and it will return the response [“spring cloud”].

Now again start the `SimpleProductService` and hit the URL in the browser `http://localhost:8080`. You will see the response as [“Eureka”, “Hystrix”, “Zuul”, “Ribbon”]. Hystrix is smart enough to detect that the `SimpleProductServiceis` up again so it directs the traffic to its normal route again.

---

## Configuring Hystrix
In the previous example, how does Hystrix know that `SimpleProductService` is down? The answer is, well pretty simple, it calls the SimpleProductService endpoints and waits for its response for a default period. If any response does not come from this service then within that time, Hystrix assumes it a failure. Now, what if you want to change this default period? Well, you can do it. You can also configure different other properties of Hystrix using commandProperties attribute of `@HystrixCommand` annotation.

With `commandProperties`, the attribute takes a value of an array of `HystrixProperty` which is, in turn, a key-value pair that are used to configure different behavior. Modify the previous example as follows

```java
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.client.RestTemplate;
import java.net.URI;

@RestController
@SpringBootApplication
@EnableCircuitBreaker
public class SimpleClientApplication {

  @GetMapping
  @HystricCommand(fallbackMethod=”defaultProductList”, commandProperties =    { @HystrixProperty(name=”execution.isolation.thread.timeoutInMilliSeconds”, value=”500”)
}) 
public String cloudProductList(@PathVariable long timeout) {

   Thread.sleep(timeout);
   RestTemplate restTemplate = new RestTemplate();
   URI uri = URI.create("http://localhost:8090/products");
   return restTemplate.getForObject(uri, String.class);

  }
public List<String> defaultProductList(long timeout) {
    return Arrays.asList("spring cloud");
  }


  public static void main(String[] args) {
    SpringApplication.run(SimpleClientApplication.class, args);
  }
}
```

`@HystricCommand(fallbackMethod=”defaultProductList”, commandProperties = { @HystrixProperty(name=”execution.isolation.thread.timeoutInMilliSeconds”, value=”500”)})`

`Thread.sleep()` will induce a delay that will be given by the user. One thing you must remember that the signature of the normal method and fallbackMethod must be the same.

Now restart both the application and hit the URL in browser with `http://localhost:8080/400`.

Since here timeout given by the user is less than timeout given in the `@HystrixProperty`, Hystrix will direct the traffic to normal route and response will be [“Eureka”, “Hystrix”, “Zuul”, “Ribbon”]. Now hit the browser with URL `http://localhost:8080/600`.

Here timeout is more than timeout is given in `@HystrixProperty` and Hystrix will consider it as a failure and redirect traffic to fallbackMethod and response will come from this `fallbackMethod` and you will see the response as [“spring cloud”].

---

## Fallback Method Chaining
Now, what happens if the fallback method itself fails? You can have a fallback method of a fallback method and in turn, this fallback method can also have its fallback method and so on.

For that, you only need to mark the fallback method by `@HystrixCommand` as below:

```java
@HystrixCommand(fallbackMethod = "zuul")
public String product() {
return getAllProductAsString();
}

@HystrixCommand(fallbackMethod = "eureka")
private String zuul() {
    return “Zuul”;
}

@HystrixCommand
private String eureka (){
return “Eureka”;
}
```

Default Fallback
If you need a common behavior whenever a failure occurs then you need not declare a separate fallback method for separate HysrixCommand.Instead, you could have only one default fallback method at class level as the following example.

```java
@DefaultProperties(defaultFallback = "fallback")
    public class SimpleProductService {
        @RequestMapping(value = "/zuul")
        @HystrixCommand
        public String zuul(){
            // some codes here
            return 
        }

        @RequestMapping(value = "/eureka")
        @HystrixCommand
        public String eureka() {
            // some codes here
            return “Eureka”
        }
        @RequestMapping(value = "/hystrix")
        @HystrixCommand
        public String hystrix() {
            // some codes here
            return “Hystrix”
        }
private String fallback() {
            return “Defalut”;
        }
    }
```
---

## Monitoring with Hystrix Dashboard

One of the strengths of Hystrix is its dashboard. The dashboard provides a real-time stream of metrics, including:
- Service performance and health
- Request counts and failure rates
- Circuit breaker status for each endpoint

This visual insight is crucial for identifying performance degradation and potential failures in a distributed system.

---

## Demonstrating Failure Recovery Scenarios

Throughout this exploration, we will simulate several failure recovery scenarios to test the robustness of our setup:
- **Service Removal:**  
  Temporarily shutting down a microservice to see Hystrix reroute traffic to the fallback.
- **Failover and Recovery:**  
  Observing how Hystrix reverts to normal operations once the failing service is back online.  
  Hystrix smartly closes the circuit when conditions improve, automatically restoring the standard request flow.

---

## Hystrix Dashboard
Hystrix dashboard allows us to monitor all the `HystrixCommand` we have set up a circuit breaker.

To enable the Hystrix dashboard you have to add `HysrixDashboard` dependency in the classpath and you have to replace the `@EnableCircuitBreaker` annotation with two annotation

`@EnableHystrix` and `@EnableHysrixDashBoard`.

Next restart both the application and go to `http://localhost:8080/hystrix`. You will see a page like this

![Micro3](/assets/img/micro3.png)

Enter the URL on this page as `http://localhost:8080/hystrix.stream` and click the button `MonitorStreamYou` will get a visual representation of metrics of all the `HystrixCommand` in your application.

![Micro4](/assets/img/micro4.png)

You can generate traffic by refreshing the `http://localhost:8080/{timeout}` and make it open or closed by increasing the timeout value less or more than the threshold value (500) and check the status of the HystrixCommand.

## Conclusion

By setting up Hystrix with your microservices, you introduce a powerful mechanism to handle failures gracefully. The circuit breaker pattern prevents cascading failures, offers configurable thresholds for error and latency management, and provides a clear monitoring dashboard for real-time insights.

This approach not only improves system resilience but also enhances the user experience by ensuring that fallback options are in place when things go wrong. Experiment with these techniques in your own applications to build more robust, fault-tolerant systems.

> Happy Coding :)