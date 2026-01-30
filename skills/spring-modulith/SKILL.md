---
name: spring-modulith
description: Expert guidance on building modular monoliths with Spring Modulith 2.0.2+ (targeting Spring Boot 3.4+). Covers module structure, verification, testing, and documentation generation.
---

# Spring Modulith Skill

This skill provides comprehensive assistance for developing applications using Spring Modulith 2.0.2+. It helps developers structure code into logical modules, enforce boundaries, and generate documentation.

## Capabilities

- **Module Structure**: Guidance on defining application modules using package structures.
- **Verification**: Enforcing architectural boundaries and preventing cyclic dependencies.
- **Testing**: Using `@ApplicationModuleTest` for isolated module integration testing.
- **Event-Driven**: Implementing loosely coupled interactions via Spring application events.
- **Documentation**: generating C4 diagrams and Module Canvas.
- **Observability**: Insight into module interactions.

## Key Concepts

1.  **Application Modules**: Functional slices of the application, defined by top-level packages.
2.  **Encapsulation**: Using Java's package-private visibility to hide internal implementation details.
3.  **Explicit APIs**: Exposing only necessary components via public classes/interfaces or `package-info.java` `@NamedInterface`.
4.  **Event Externalization**: Handling transactional events and reliable publication.

## Common Tasks

### Defining a Module
Modules are implicitly defined by direct sub-packages of the main application package.

```java
package com.example.inventory; // Module 'inventory'

@Service
class InventoryService { ... } // Internal component (package-private)
```

### Verifying Structure
Ensure modules adhere to constraints.

```java
@Test
void verifyModularity() {
    ApplicationModules.of(Application.class).verify();
}
```

### Module Integration Test
Test a module in isolation (with its dependencies mocked or bootstrapped).

```java
@ApplicationModuleTest
class InventoryModuleTests {
    @Test
    void verifiesModuleStructure(Scenario scenario) {
        // ...
    }
}
```

## References
This skill loads detailed documentation on specific topics:
- `references/spring-modulith.md`: General overview.
- `references/spring-modulith-architecture.md`: Architecture guidelines.
- `references/spring-modulith-best-practices.md`: Design patterns and dos/donts.
- `references/spring-modulith-documentation.md`: Generating documentation (C4, Canvas).
- `references/spring-modulith-event-publication.md`: Event publication and handling.
- `references/spring-modulith-examples.md`: Code examples.
- `references/spring-modulith-maven-gradle.md`: Build tool configuration.
- `references/spring-modulith-observability.md`: Observability and tracing.
- `references/spring-modulith-testing.md`: Detailed testing guide.
