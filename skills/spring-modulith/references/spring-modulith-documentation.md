# Spring Modulith Documentation Generation

Spring Modulith can generate documentation based on the code structure. "Living Documentation" ensures docs never rot.

## Generating Diagrams

You can generate C4 component diagrams (PlantUML) during the test phase.

```java
class DocumentationTests {

    @Test
    void writeDocumentationSnippets() {

        var modules = ApplicationModules.of(Application.class);

        new Documenter(modules)
            .writeModulesAsPlantUml()
            .writeIndividualModulesAsPlantUml();
    }
}
```

This outputs `.puml` files to `target/spring-modulith-docs`.

## Module Canvas

The Module Canvas is a high-level summary of a module, including:
-   Aggregates (Entities)
-   Events listened to / published
-   Beans exposed
-   Dependencies

```java
new Documenter(modules).writeModuleCanvases();
```

## Integration with Asciidoctor

You can include these generated snippets in a master `index.adoc` file.

```asciidoc
= System Documentation

== Modules

include::target/spring-modulith-docs/components.puml[]
```

This allows you to build a comprehensive architectural handbook that updates every time you run tests.
