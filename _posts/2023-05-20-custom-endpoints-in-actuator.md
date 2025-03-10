---
date: 2023-05-20 00:01:00 +0530
layout: post
title: Custom End Points - Spring Actuator
subtitle: Custom end points in Spring actuator.
description: Custom end points in Spring actuator. 
image: /assets/img/spring-boot-actuator-custom.png
optimized_image: /assets/img/spring-boot-actuator-custom.png
category: code
tags:
  - spring
  - java
author: ankurmalviya1618
---
# Custom Endpoints in Spring boot Actuator

**Custom Endpoints in Spring Boot Actuator** extends the default functionality provided by the Actuator. It involves defining a custom endpoint class annotated with `@Endpoint`, which contains methods annotated with `@ReadOperation`, `@WriteOperation`, and `@DeleteOperation` to handle `GET`, `POST`, and `DELETE` requests. **Spring Boot Actuator** allows you to expose these custom endpoints through `HTTP` by configuring the necessary security settings. Using the `SecurityFilterChain`, we can secure these endpoints to ensure that only authorized users can access them.

## Key Annotations:
- `@Endpoint`: It marks a class as the endpoint to be exposed by the Spring Boot Actuator. This annotation can be used on the class level to define the Custom Actuator endpoint.
- `@ReadOperation`: It indicates that the annotated method should handle the GET requests for the custom endpoint. This annotation can be used on methods within the @Endpoint class to define the read (GET) operations.
- `@WriteOperation`: It indicates that the annotated method should handle the POST requests for the custom endpoint. This annotation can be used on methods within the @Endpoint class to define the write (POST) operations.
- `@DeleteOperation`: It indicates that the annotated method should handle the DELETE requests for the custom endpoint. This annotation can be used on methods within the @Endpoint class to define the delete(DELETE) operations.

## Implementation of Custom Endpoints in Spring Boot Actuator
Below are the steps to implement Custom Endpoints in Spring boot Actuator.

### 1. Create the Spring Project

Create a new Spring Boot project using Spring Initializr and add below dependencies,

- Spring Web
- Spring Security
- Spring Boot Actuator
- Lombok
- Spring DevTools

After this step, the project structure will be like below.

![Actuator1](/assets/img/actuator1.png)

### 2. Configure the Application properties

Open the application.properties file and add the properties.

```properties
spring.application.name=custom-endpoints-actuators

management.endpoints.web.exposure.include=*
management.endpoint.customEndpoint.enabled=true

# Enable security for actuator endpoints
management.endpoint.customEndpoint.roles=ACTUATOR
```

### 3. Configure Web Security
We will create the security configuration class to secure the custom endpoints and other actuator endpoints of the application.

Go to **src > main > java > org.example.customendpointsacuators > SecurityConfig** and put the below code.

```java
package org.example.customendpointsactuators;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

import static org.springframework.security.config.Customizer.withDefaults;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                .authorizeRequests(authorizeRequests ->
                        authorizeRequests
                                .requestMatchers("/actuator/**").hasRole("ACTUATOR")
                                .anyRequest().authenticated()
                )
                .httpBasic(withDefaults())
                .csrf(AbstractHttpConfigurer::disable);

        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails user = User.builder()
                .username("user")
                .password("{noop}password")
                .roles("ACTUATOR")
                .build();
        return new InMemoryUserDetailsManager(user);
    }
}
```

### 4. Create Custom Endpoints
We will create the custom endpoints class with the methods for the different operations of the application.

Go to **src > main > java > org.example.customendpointsacuators > CustomEndpoints** and put the below code.

```java
package org.example.customendpointsactuators;

import org.springframework.boot.actuate.endpoint.annotation.DeleteOperation;
import org.springframework.boot.actuate.endpoint.annotation.Endpoint;
import org.springframework.boot.actuate.endpoint.annotation.ReadOperation;
import org.springframework.boot.actuate.endpoint.annotation.WriteOperation;
import org.springframework.stereotype.Component;

@Component
@Endpoint(id = "customEndpoint")
public class CustomEndpoint {

    @ReadOperation
    public String customEndpoint() {
        return "Hello from custom endpoint!";
    }

    @WriteOperation
    public String writeOperation(String name) {
        return "Hello, " + name;
    }

    @DeleteOperation
    public String deleteOperation() {
        return "Delete operation performed";
    }
}
```

### 5. Main Class
No changes are required in the main class.

```java
package org.example.customendpointsactuators;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class CustomEndpointsActuatorsApplication {

    public static void main(String[] args) {
        SpringApplication.run(CustomEndpointsActuatorsApplication.class, args);
    }

}
```

**pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.3.0</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>org.example</groupId>
    <artifactId>custom-endpoints-actuators</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>custom-endpoints-actuators</name>
    <description>custom-endpoints-actuators</description>
    <properties>
        <java.version>17</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.security</groupId>
            <artifactId>spring-security-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>
```

### 6. Run the application

Now, we will run the application as a spring then it will be start port at `8080`.

![Actuator2](/assets/img/actuator2.png)

### 7. Testing the Custom Endpoints
Authorization: Set the Basic Auth and provide the username and password.

**Testing the Read Operation (GET):**

```bash
GET  http://localhost:8080/actuator/customEndpoint
```

**Output:**

![Actuator3](/assets/img/actuator3.png)

**Testing the Write Operation (POST):**

```bash
POST http://localhost:8080/actuator/customEndpoint
```

**Output:**

![Actuator4](/assets/img/actuator4.png)

**Testing the Delete Operation (DELETE):**

```bash
DELETE http://localhost:8080/actuator/customEndpoint
```

**Output:**

![Actuator5](/assets/img/actuator5.png)

> Happy Coding :)








