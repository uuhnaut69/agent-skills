# Spring Modulith Architecture

Spring Modulith enforces logical architecture within a single deployment unit.

## The Architectural onion

While "Onion Architecture" usually refers to layers (Domain, Application, Infra), Spring Modulith applies this to **Modules**.

1.  **Application Domain**: The core set of modules implementing business capabilities.
2.  **Infrastructure**: Adapters that talk to the outside world (Web, Persistence).

In Spring Modulith, the **Application Module** is the primary unit.

## Verification API

Spring Modulith uses `ArchUnit` under the hood to verify dependencies.

```java
ApplicationModules modules = ApplicationModules.of(Application.class);

// 1. Verify standard modularity rules (no cycles, no illegal access)
modules.verify();
```

## Cycle Detection

Cycles between modules (A -> B -> A) are forbidden.
If you have a cycle, you usually need to:
1.  **Invert Dependency**: Use Events. A publishes event, B listens. B no longer depends on A directly (or vice versa).
2.  **Extract Common**: Move shared logic to a third module C. A -> C, B -> C.

## Named Interfaces

Sometimes a module needs to expose multiple "APIs" for different consumers.
-   `SPI` (Service Provider Interface)
-   `API` (Client facing)

You can use `@NamedInterface` on a package-info.java file to define these slices.

```java
@NamedInterface("spi")
package com.acme.myproject.module.spi;
```

Modules can then declare which named interface they require (though this is advanced usage, usually simple public/package-private separation is enough).
