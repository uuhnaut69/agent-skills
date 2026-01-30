# Maven & Gradle Configuration

To use Spring Modulith, you need to manage dependencies properly.

## BOM (Bill of Materials)

Spring Modulith provides a BOM to manage versions.

### Maven

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.modulith</groupId>
            <artifactId>spring-modulith-bom</artifactId>
            <version>2.0.2</version> <!-- Use latest version -->
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<dependencies>
    <dependency>
        <groupId>org.springframework.modulith</groupId>
        <artifactId>spring-modulith-starter-core</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.modulith</groupId>
        <artifactId>spring-modulith-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### Gradle

```groovy
plugins {
    id 'io.spring.dependency-management' version '1.1.4'
}

dependencyManagement {
    imports {
        mavenBom "org.springframework.modulith:spring-modulith-bom:2.0.2"
    }
}

dependencies {
    implementation 'org.springframework.modulith:spring-modulith-starter-core'
    testImplementation 'org.springframework.modulith:spring-modulith-starter-test'
}
```

## Observability & Persistence

For Event Publication Registry, add the specific starter:

-   `org.springframework.modulith:spring-modulith-starter-jpa`
-   `org.springframework.modulith:spring-modulith-starter-jdbc`
-   `org.springframework.modulith:spring-modulith-starter-mongodb`

For Actuator/Observability:

-   `org.springframework.modulith:spring-modulith-actuator`
-   `org.springframework.modulith:spring-modulith-observability`
