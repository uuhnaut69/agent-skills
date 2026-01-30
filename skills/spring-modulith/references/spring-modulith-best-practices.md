# Spring Modulith Best Practices

## 1. Package Structure

### Do
-   Keep the root application class in the root package.
-   Create functional top-level packages for your modules (e.g., `sales`, `catalog`, `shipping`).
-   Use sub-packages within modules for technical separation if needed (e.g., `sales.web`, `sales.persistence`), but keep the logical module cohesive.

### Don't
-   Group by layer at the top level (e.g., `controllers`, `services`, `repositories`). This is "Screaming Architecture" violation.
-   Create circular dependencies between modules.

## 2. Visibility modifiers

### Do
-   Make implementation classes **package-private** (`default` visibility) whenever possible.
-   Only make classes `public` if they are part of the module's API exposed to other modules.
-   Use `package-info.java` with `@org.springframework.modulith.NamedInterface` to expose specific sub-packages if your module has internal structure.

### Don't
-   Make everything `public`. This defeats the purpose of modularity.

## 3. Inter-Module Communication

### Do
-   Use **Spring Events** (`ApplicationEventPublisher`) for side effects (e.g., "When Order Placed, Update Inventory").
-   Use direct method calls (injecting beans) only for synchronous, improved consistency requirements, but keep the API surface small.
-   Use `@Async` for event listeners to decouple execution time.

### Don't
-   Share JPA Entities across modules. Each module should own its data model. If modules need to share data, use DTOs or reference by ID.
-   Inject Repository interfaces from one module into another. Access data via the Service layer.

## 4. Testing

### Do
-   Use `@ApplicationModuleTest` to test modules in isolation.
-   Verify the architectural structure in a dedicated test.

## 5. Event Publication Registry

If you use events for critical business logic between modules, ensure you use the **Event Publication Registry** (backed by JPA, JDBC, or MongoDB) to guarantee event delivery (Outbox Pattern) in case of listener failure.
