# Spring Modulith Testing

## @ApplicationModuleTest

This annotation is the cornerstone of integration testing in Spring Modulith. It enables a "slice test" for a specific module.

### How it works
1.  It bootstraps the Spring Context.
2.  It includes all beans defined *within* the module under test.
3.  It *mocks* beans from other modules automatically (if they are required), or you can control how cross-module dependencies are satisfied.
4.  It enables the `Scenario` API for testing async event flows.

### Example

```java
package com.example.shop.order;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.modulith.test.ApplicationModuleTest;
import org.springframework.modulith.test.Scenario;

@ApplicationModuleTest
class OrderModuleIntegrationTest {

    @Autowired OrderService orderService;

    @Test
    void publishesEventOnCompletion(Scenario scenario) {

        scenario.stimulate(() -> orderService.completeOrder(new OrderId("123")))
                .andWaitForEventOfType(OrderCompleted.class)
                .matching(event -> event.orderId().value().equals("123"))
                .toArrive();
    }
}
```

## Structural Verification Test

You should have at least one test in your suite that verifies the overall architecture.

```java
class ArchitectureTests {
    @Test
    void verifyModularity() {
        ApplicationModules.of(Application.class).verify();
    }
}
```

## Testing Events

The `Scenario` DSL provided by `spring-modulith-test` makes testing async events deterministic.
-   `stimulate(() -> ...)`: Trigger the action.
-   `andWaitForEventOfType(...)`: Wait for the event listener to execute.
-   `toArrive()`: Assert it happens.

This removes the need for `Thread.sleep()` in tests.
