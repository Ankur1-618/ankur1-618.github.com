---
date: 2023-08-31 00:30:00 +0530
layout: post
title: Spring Boot Actuator
subtitle: Spring Boot - How to use actuator
description: Spring Boot - How to use actuator
image: /assets/img/spring-boot-actuators-banner.jpg
optimized_image: /assets/img/spring-boot-actuators-banner.jpg
category: java
tags:
  - java
  - spring
author: ankurmalviya1618
my_prop: 'nil'
---
# Harnessing Spring Boot Actuator for Enhanced Application Monitoring and Management

Spring Boot Actuator is an essential tool for developers looking to gain deep insights into their applications. It provides production-ready features such as monitoring, metrics, health checks, and auditing out of the box. In this blog, we'll explore the key features of Spring Boot Actuator and see examples of how to use them.

## What is Spring Boot Actuator?

Spring Boot Actuator adds several production-ready endpoints to your application that let you monitor and manage your app in real time. These endpoints provide critical information about your application’s health, configuration, metrics, and environment.

- With the help of Spring Boot, we can achieve the above objectives.
- Spring Boot’s ‘Actuator’ dependency is used to monitor and manage the Spring web application.
- We can use it to monitor and manage the application with the help of HTTP endpoints or with the JMX.

<img src="/assets/img/spring-boot-actuator.jpg" alt="Spring Boot Actuator Image">

### Advantages of Actuator the Application
- It increases customer satisfaction.
- It reduces downtime.
- It boosts productivity.
- It improves Cybersecurity Management.
- It increases the conversion rate.

## Key Features of Spring Boot Actuator

### 1. Health Checks

One of the most common endpoints is `/actuator/health`. This endpoint provides a quick view of your application’s health status.

**Example:**

Add the dependency to your `pom.xml` or `build.gradle`:

```xml
<!-- For Maven -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

After starting your Spring Boot application, access:

```bash
http://localhost:8080/actuator/health
```

The response might look like:

```json
{
  "status": "UP"
}
```

You can also customize the health check by creating a custom `HealthIndicator`.

```java
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.stereotype.Component;

@Component
public class CustomHealthIndicator implements HealthIndicator {
    @Override
    public Health health() {
        // Add your custom logic here
        boolean healthy = checkSomething();
        if (healthy) {
            return Health.up().withDetail("CustomHealth", "Everything is OK!").build();
        }
        return Health.down().withDetail("CustomHealth", "Something went wrong!").build();
    }
    
    private boolean checkSomething() {
        // Simulate a health check
        return true;
    }
}
```

### 2. Application Info

The `/actuator/info` endpoint provides general information about your application. You can customize it via your `application.properties` or `application.yml`.

**Example:**

Add properties to your `application.yml`:

```yaml
management:
  endpoints:
    web:
      exposure:
        include: info, health, metrics, env
info:
  app:
    name: Spring Boot Actuator Demo
    version: 1.0.0
```

Access the endpoint at:

```bash
http://localhost:8080/actuator/info
```

Expected output:

```json
{
  "app": {
    "name": "Spring Boot Actuator Demo",
    "version": "1.0.0"
  }
}
```

### 3. Metrics and Monitoring
   Spring Boot Actuator integrates with Micrometer to expose application metrics. The /actuator/metrics endpoint gives you detailed metrics such as JVM memory usage, HTTP request counts, and more.

**Example:**

Access:
```bash
http://localhost:8080/actuator/metrics
```

To get a specific metric, such as the number of HTTP requests, access:
```bash
http://localhost:8080/actuator/metrics/http.server.requests
```

You can further integrate these metrics with monitoring tools like Prometheus, Graphite, or New Relic by adding the respective Micrometer registry dependency.

### 4. Custom Endpoints
   You can also create custom endpoints if you need to expose specific information or management functions.

**Example:**

```java
import org.springframework.boot.actuate.endpoint.annotation.Endpoint;
import org.springframework.boot.actuate.endpoint.annotation.ReadOperation;
import org.springframework.stereotype.Component;

@Component
@Endpoint(id = "custom")
public class CustomEndpoint {

    @ReadOperation
    public String customEndpoint() {
        return "This is a custom endpoint";
    }
}
```

Access the custom endpoint at:

```bash
http://localhost:8080/actuator/custom
```

### 5. Environment and Configuration Details
   The `/actuator/env` endpoint displays the current environment properties, while `/actuator/configprops` shows all configuration properties. These endpoints are useful for debugging and ensuring the correct configuration in production.

### 6. Security Considerations
   Actuator endpoints provide sensitive data and control operations. It’s important to secure them in production environments. You can restrict access by configuring security settings in your `application.properties` or using Spring Security.

**Example:**

```yaml
management:
  endpoints:
    web:
      exposure:
        include: health, info, metrics
  security:
    roles: ACTUATOR
```

Then, secure the endpoints using standard Spring Security configuration.

## Conclusion
Spring Boot Actuator is a robust tool that offers a plethora of features for monitoring and managing your application in production. Whether it’s health checks, application info, metrics, custom endpoints, or environment details, Actuator provides an easy way to gain insights and ensure your application is running smoothly.

By integrating Actuator with external monitoring systems, you can build a comprehensive solution to track and improve your application's performance and reliability.

> Happy Coding :)