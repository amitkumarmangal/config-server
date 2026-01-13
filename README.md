# Config Server

## 📌 Overview

This repository contains the **Spring Cloud Config Server**, which provides **centralized external configuration management** for all microservices.

The Config Server allows applications to:

* Load configuration from a central Git repository
* Change configuration without rebuilding services
* Support multiple environments (dev / test / prod)
* Secure and version-control application configuration

---

## 🎯 Key Responsibilities

* Centralized configuration management
* Environment-specific configuration delivery
* Dynamic configuration refresh
* Git-backed configuration versioning
* Secure configuration access

---

## 🧱 Architecture Role

```
┌──────────────┐     Fetch Config      ┌─────────────────┐
│  Service A   │ ───────────────────▶ │                 │
├──────────────┤                       │  Config Server  │
│  Service B   │ ───────────────────▶ │                 │
├──────────────┤                       │                 │
│  Service C   │ ◀─────────────────── │  Git Repository │
└──────────────┘     Config Files      └─────────────────┘
```

---

## 🛠 Technology Stack

* Java 17
* Spring Boot
* Spring Cloud Config Server
* Maven
* Git (GitHub )

---

## 🚀 Running the Config Server

### 1️⃣ Build the Application

```bash
mvn clean package
```

---

### 2️⃣ Run Locally

```bash
mvn spring-boot:run
```

or

```bash
java -jar target/config-server.jar
```

---

## 🌐 Default Access

```
http://localhost:8888
```

---

## ⚙ Configuration

### `application.yml`

```yaml
server:
  port: 8888

spring:
  application:
    name: config-server

  cloud:
    config:
      server:
        git:
          uri: https://github.com/amitkumarmangal/config-repo
          default-label: main
          clone-on-start: true
```
---

## 📂 Configuration Repository Structure

Example Git config repo:

```
config-repo/
├── application.yml
├── application-dev.yml
├── application-prod.yml
├── order-service.yml
├── order-service-dev.yml
└── order-service-prod.yml
```

---

## 🔍 How Clients Fetch Configuration

### Configuration URL Format

```
/{application}/{profile}/{label}
```

Examples:

```
/order-service/dev
/order-service/prod/main
/application/default
```

---

## 🔗 Client Configuration

### Add Dependency

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-config</artifactId>
</dependency>
```

---

### `bootstrap.yml` or `application.yml`

```yaml
spring:
  application:
    name: order-service
  config:
    import: optional:configserver:http://localhost:8888
```

---

## 🔄 Dynamic Refresh (Optional)

Enable Actuator:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

Trigger refresh:

```bash
POST /actuator/refresh
```

(Requires Spring Cloud Bus for multi-instance refresh)

---
## 🧠 Best Practices

* Do not store secrets in plain text
* Use encrypted values or Vault integration
* Use environment-specific config files
* Version control all configuration changes
* Restrict access to Config Server

---

## 🚨 Common Issues

| Issue                 | Cause             | Fix                             |
| --------------------- | ----------------- | ------------------------------- |
| Config not loading    | Wrong app name    | Match `spring.application.name` |
| Git auth failure      | Invalid token     | Verify credentials              |
| Config not refreshing | Actuator disabled | Enable `/refresh`               |
| Slow startup          | Git clone delay   | Enable `clone-on-start`         |

---

## 👥 Ownership & Support

Maintained by the **Platform / Architecture Team**.

For:

* Configuration changes
* Access issues
* Production incidents

Please raise a Pull Request or contact the platform team.

---

## 📄 License

Internal use only.
Not intended for public distribution.
