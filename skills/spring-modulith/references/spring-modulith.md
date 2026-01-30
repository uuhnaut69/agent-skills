# Spring Modulith 2.0.2+ Overview

Spring Modulith provides extensions to Spring Boot to build modular monolithic applications. It focuses on the structural validation and documentation of Spring Boot applications.

## Core Philosophy

Spring Modulith helps developers keep their monolithic applications maintainable by encouraging:
1.  **Strong Encapsulation**: Hiding implementation details within modules.
2.  **Explicit Dependencies**: Making module interactions visible and verifiable.
3.  **Event-Driven Interaction**: Reducing coupling through messaging.

## Application Modules

An **Application Module** is a unit of functionality that consists of:
-   A specific package (and its sub-packages).
-   A set of exposed components (API).
-   A set of internal components (Implementation).
-   Explicit dependencies on other modules.

### Structure
By default, Spring Modulith considers the direct sub-packages of the application's main package as modules.

```
com.acme.myproject
├── Application.java
├── customer            <-- Module 'customer'
│   ├── Customer.java
│   └── ...
├── order               <-- Module 'order'
│   ├── Order.java
│   └── ...
└── inventory           <-- Module 'inventory'
    ├── Inventory.java
    └── ...
```

## Dependency Injection & Boundaries

Spring Modulith analyzes the bean dependency graph.
-   **Allowed**: A bean in Module A can inject a public bean from Module B *if* Module A is allowed to depend on Module B.
-   **Disallowed**: A bean in Module A cannot inject an internal (package-private) bean from Module B.
-   **Cyclic Dependencies**: Spring Modulith prohibits cyclic dependencies between modules by default.

## Main Features

1.  **Verification**: Fails the build/test if architectural rules are violated.
2.  **Documentation**: Generates Component, Container, and Module Canvas diagrams.
3.  **Integration Testing**: Bootstraps only the specific module under test.
4.  **Moments**: Event externalization (e.g., to Kafka/RabbitMQ) with the "Outbox Pattern".
