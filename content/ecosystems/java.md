+++
archetype = "chapter"
title = "Java"
weight = 3.3
+++

Java Stacks Projects are driven via Maven based workflows, predomanantly this means that each project has a `pom.xml` which will have a series of variables:

```xml
<applicationinsights.version>2.6.4</applicationinsights.version>
<azure.springboot.version>4.0.0</azure.springboot.version>
<au.com.dius.pact-jvm-provider-spring.version>4.0.10</au.com.dius.pact-jvm-provider-spring.version>
<au.com.dius.pact.consumer-version>4.6.15</au.com.dius.pact.consumer-version>
<au.com.dius.pact.provider.maven-version>4.6.15</au.com.dius.pact.provider.maven-version>
<aws-java-sdk-s3.version>1.12.779</aws-java-sdk-s3.version>
<aspectjweaver.version>1.9.9.1</aspectjweaver.version>
<exec-maven-plugin.version>3.5.0</exec-maven-plugin.version>
<spring.cloud.dependencies.version>2022.0.4</spring.cloud.dependencies.version>
<pact.version>3.5.24</pact.version>
<spring.data.commons>3.4.0</spring.data.commons>
<owasp-dependency-check-plugin.version>12.1.3</owasp-dependency-check-plugin.version>
<junit-jupiter.version>5.11.3</junit-jupiter.version>
```

Allowing central control of the package versions in use across the Java Project; for example to update the version of OWASP Dependency Check to `12.1.4` in the build pipeline it's nessecary to bump the version number to latest and then rebuild.

## Local Testing

It's possible to build and test locally as long as you have the relevant Java version installed:

```sh
cd java
./mvnw clean install test -Plocal
```
