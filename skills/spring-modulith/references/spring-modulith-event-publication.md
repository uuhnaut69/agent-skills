# Spring Modulith Event Publication

Implementing "Event-Driven Architecture" correctly is hard. Spring Modulith provides the **Event Publication Registry** to solve the "Dual Write Problem" (updating DB and sending a message).

## The Problem
If you save an Order and publish an Event, and the Event Listener fails (or the broker is down), the Order is saved but the side effects didn't happen. System is inconsistent.

## The Solution: Outbox Pattern
Spring Modulith intercepts event publication.
1.  When an event is published within a transaction, it serializes the event to a log (table in DB).
2.  The transaction commits (Business data + Event log committed atomically).
3.  An asynchronous listener picks up the event.
4.  If the listener succeeds, the log entry is marked completed.
5.  If the listener fails, it can be retried.

## Configuration

Add the starter:
`spring-modulith-starter-jpa` (or `jdbc`, `mongodb`).

It automatically creates the necessary schema (e.g., `EVENT_PUBLICATION` table).

## Usage

Just use standard `ApplicationEventPublisher`.

```java
@Transactional
public void doSomething() {
    repo.save(entity);
    events.publishEvent(new MyEvent()); // Will be captured by Registry
}
```

## Externalization (Kafka/Rabbit/JMS)

To forward these internal events to an external broker:

```java
@Component
class OrderEvents {

    // Listen to internal event, forward to external
    @ApplicationModuleListener
    void on(OrderCompleted event) {
        kafkaTemplate.send("orders", event);
    }
}
```

Spring Modulith ensures this listener is executed reliably.
