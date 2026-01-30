# Spring Modulith Examples

## 1. Module Definition

```java
// src/main/java/com/example/shop/order/OrderService.java
package com.example.shop.order;

import org.springframework.stereotype.Service;
import com.example.shop.inventory.InventoryService; // Allowed if public

@Service
public class OrderService {

    private final InventoryService inventory;

    // Constructor injection
    public OrderService(InventoryService inventory) {
        this.inventory = inventory;
    }

    public void placeOrder(Order order) {
        // ... logic
    }
}
```

## 2. Event Publication

```java
// src/main/java/com/example/shop/order/OrderService.java
package com.example.shop.order;

import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
class OrderService {

    private final ApplicationEventPublisher events;
    private final OrderRepository orders;

    // ... constructor

    @Transactional
    public void completeOrder(OrderIdentifier id) {
        var order = orders.findById(id).orElseThrow();
        order.complete();
        orders.save(order);

        // Publish event
        events.publishEvent(new OrderCompleted(id));
    }
}
```

## 3. Event Listener (Async)

```java
// src/main/java/com/example/shop/inventory/InventoryListener.java
package com.example.shop.inventory;

import org.springframework.modulith.events.ApplicationModuleListener;
import org.springframework.stereotype.Component;

@Component
class InventoryListener {

    // @ApplicationModuleListener is alias for @Async + @TransactionalEventListener
    @ApplicationModuleListener
    void on(OrderCompleted event) {
        // Update inventory
        System.out.println("Updating inventory for order: " + event.orderId());
    }
}
```

## 4. Named Interfaces (Exposing sub-packages)

```java
// src/main/java/com/example/shop/inventory/api/package-info.java
@org.springframework.modulith.NamedInterface("api")
package com.example.shop.inventory.api;
```

Other modules can now refer to this named interface explicitly, or Spring Modulith can control access.
