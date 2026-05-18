# MDS Platform

![Java](https://img.shields.io/badge/Java-21-orange)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-4.0.6-brightgreen)
![Spring Cloud](https://img.shields.io/badge/Spring_Cloud-2025.1.1-brightgreen)
![Maven](https://img.shields.io/badge/Maven-3.9-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-success)
![Platform](https://img.shields.io/badge/MDS-Platform-blueviolet)

Parent platform for MDS (Martins Desenvolvimento de Sistemas), responsible for centralizing dependency management, version control, build standards, plugins, and shared configurations across the ecosystem.

---

## Overview

MDS Platform acts as the central governance layer of the ecosystem. It is a **POM-only** project (no Java source code) that every MDS library and application inherits from.

This project manages:

- Dependency versions (MDS + third-party)
- Maven `dependencyManagement`
- Plugin versions and configurations
- Java 21 compilation standards
- Spring Boot 4.0.6 + Spring Cloud 2025.1.1 baseline
- Build patterns (encoding, source JARs, code formatting)
- Governance rules (minimum Java/Maven versions via enforcer)

---

## Technical Stack

- Java 21
- Spring Boot 4.0.6
- Spring Cloud 2025.1.1 (Oakwood)
- Maven 3.9+
- Jackson 3.x (via Spring Boot parent)

---

## Architecture

```text
spring-boot-starter-parent 4.0.6
    ↓ parent
MDS Platform (governance parent POM)
    ↓ dependencyManagement + pluginManagement
    ├── shared-core-lib .............. foundational utilities, patterns & helpers
    ├── spring-error-pattern ........ exception hierarchy & error handling
    ├── spring-retry-pattern ........ retry policies
    ├── spring-crypto-pattern ....... encryption & hashing
    ├── spring-token-pattern ........ SSO token management
    ├── spring-comm-pattern ......... Feign HTTP client abstractions
    ├── spring-security-pattern ..... security filters & CORS
    ├── spring-data-pattern ......... data access abstractions
    ├── spring-cache-pattern ........ multicache (Caffeine + Redis)
    ├── spring-crud-pattern ......... CRUD base classes
    └── mds-platform-starter ....... Spring Boot starter (aggregates all above)
```

---

## Project Structure

```
mds-platform/
├── pom.xml ............. governance parent POM (dependencyManagement + pluginManagement)
├── .gitignore .......... standard Maven gitignore
└── README.md ........... this file
```

> **Note:** This is a POM-only project. It has no `src/` directory — its purpose is to centralize versions and build standards, not to contain Java code.

---

## Managed Versions

### MDS Ecosystem

| Property | Version | Libraries |
|----------|---------|-----------|
| `mds.version` | `0.0.1-SNAPSHOT` | shared-core-lib, spring-error-pattern, spring-retry-pattern, spring-crypto-pattern, spring-token-pattern, spring-security-pattern, spring-data-pattern, spring-comm-pattern, spring-cache-pattern, spring-crud-pattern |

### Spring

| Property | Version |
|----------|---------|
| `spring-boot` (parent) | `4.0.6` |
| `spring-cloud.version` | `2025.1.1` |

### Third-party

| Property | Version | Used by |
|----------|---------|---------|
| `feign-okhttp.version` | `13.4` | spring-comm-pattern |
| `google-guava.version` | `33.1.0-jre` | spring-comm-pattern |
| `java-jwt.version` | `4.4.0` | spring-crypto-pattern |
| `commons-io.version` | `2.16.1` | spring-security-pattern |
| `owasp-encoder.version` | `1.3.1` | spring-security-pattern |
| `springdoc-openapi.version` | `2.6.0` | spring-security-pattern |

### Plugins

| Property | Version |
|----------|---------|
| `fmt-maven-plugin.version` | `2.13` |
| `maven-enforcer-plugin.version` | `3.5.0` |
| `maven-source-plugin.version` | `3.3.1` |

---

## Governance

### Enforcer Rules

The platform defines enforcer rules (via `maven-enforcer-plugin`) that are inherited by all child modules:

- **Java 21+** required
- **Maven 3.9+** required

### Plugin Management

All child modules inherit pre-configured plugins:

| Plugin | Purpose |
|--------|---------|
| `maven-compiler-plugin` | Java 21, `-parameters` flag enabled |
| `maven-surefire-plugin` | Test execution |
| `maven-javadoc-plugin` | Javadoc generation |
| `maven-source-plugin` | Source JAR attachment (`jar-no-fork`) |
| `fmt-maven-plugin` | Google Java Format (disabled by default, `skip=true`) |
| `maven-enforcer-plugin` | Governance rules enforcement |

---

## Installation

Add MDS Platform as the parent of your project:

```xml
<parent>
   <groupId>com.mds.platform</groupId>
   <artifactId>mds-platform</artifactId>
   <version>1.0.0-SNAPSHOT</version>
   <relativePath/>
</parent>
```

After inheriting, your project automatically gets:

1. All `dependencyManagement` entries — declare MDS libs without `<version>`
2. All `pluginManagement` — use plugins without version/config duplication
3. Encoding, Java version, and enforcer rules

---

## Migration from direct spring-boot-starter-parent

If your MDS library currently uses `spring-boot-starter-parent` directly:

1. Replace the `<parent>` block:

```xml
<!-- Before -->
<parent>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-parent</artifactId>
   <version>4.0.6</version>
   <relativePath/>
</parent>

<!-- After -->
<parent>
   <groupId>com.mds.platform</groupId>
   <artifactId>mds-platform</artifactId>
   <version>1.0.0-SNAPSHOT</version>
   <relativePath/>
</parent>
```

2. Remove duplicated properties (`java.version`, `spring-cloud.version`, `fmt-maven-plugin.version`, etc.) — they are inherited.

3. Remove `<version>` from MDS cross-dependencies — they are governed by `dependencyManagement`.

4. Remove `<version>` from third-party dependencies managed by the platform (feign-okhttp, guava, java-jwt, commons-io, owasp-encoder, springdoc-openapi).

5. Remove duplicated `<pluginManagement>` entries for compiler, surefire, javadoc, and fmt — they are inherited.

6. Remove local `spring-cloud-dependencies` BOM import if present — it is inherited.

---

## Future Roadmap

- BOM support (standalone BOM artifact for non-child consumers)
- Version catalogs
- Release automation
- Internal artifact registry
- Dependency convergence enforcer rule
- JaCoCo coverage enforcement

---

Built with ❤️ by Martins Desenvolvimento de Sistemas
