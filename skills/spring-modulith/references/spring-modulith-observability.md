# Spring Modulith Observability

Spring Modulith integrates with Micrometer and Spring Boot Actuator to provide visibility into module interactions.

## Runtime Visualization

Spring Modulith can track which modules call which other modules at runtime.

### Actuator Endpoint

If you include `spring-modulith-actuator` and `spring-boot-starter-actuator`, you verify the application structure at runtime via HTTP.

## Tracing (Zipkin/Brave/Otel)

When using Spring Modulith's Event Publication Registry, the processing of events is traced.
Since `@ApplicationModuleListener` runs asynchronously, standard tracing propagation is handled so you can see the causal relationship between:
1.  The HTTP request that triggered the action.
2.  The database transaction.
3.  The event publication.
4.  The async listener execution.

## Module Canvas (Runtime)

The Module Canvas (documentation) can be exposed as an Actuator endpoint, providing a live view of the system structure.
