# MDS Platform

![Java](https://img.shields.io/badge/Java-17-orange)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-brightgreen)
![Maven](https://img.shields.io/badge/Maven-3.9-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-success)
![Platform](https://img.shields.io/badge/MDS-Platform-blueviolet)

Parent platform for MDS (Martins Desenvolvimento de Sistemas), responsible for centralizing dependency management, version control, build standards, plugins, and shared configurations across the ecosystem.

---

## Overview

MDS Platform acts as the central governance layer of the ecosystem.

This project manages:

- Dependency versions
- Maven dependencyManagement
- Plugin versions
- Java standards
- Spring Boot standards
- Shared configurations
- Build patterns
- Release standards

---

## Architecture

MDS Platform
    ↓
shared-core-lib
    ↓
spring-error-pattern
spring-comm-pattern
spring-retry-pattern
spring-token-pattern
spring-security-pattern
spring-crypto-pattern
spring-data-pattern
spring-cache-pattern
spring-crud-pattern

---

## Installation

```xml
<parent>
   <groupId>br.com.mds.platform</groupId>
   <artifactId>mds-platform</artifactId>
   <version>1.0.0</version>
</parent>
```

---

## Future Roadmap

- BOM support
- Version catalogs
- Centralized plugin management
- Release automation
- Platform governance
- Internal artifact registry

---

Built with ❤️ by Martins Desenvolvimento de Sistemas
