# feign-eureka

[![Java CI with Maven](https://github.com/hendisantika/feign-eureka/actions/workflows/maven.yml/badge.svg)](https://github.com/hendisantika/feign-eureka/actions/workflows/maven.yml)
![Java](https://img.shields.io/badge/Java-25-orange?logo=openjdk)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-4.1.2-brightgreen?logo=springboot)
![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-2025.1.4-brightgreen?logo=spring)

Example of using **Spring Cloud OpenFeign** with **Netflix Eureka** service discovery.

## Architecture

```
                  ┌───────────────────────┐
                  │     eureka-server     │
                  │   :8761 (registry)    │
                  └───▲───────────────▲───┘
             register │               │ register + discover
                      │               │
┌─────────────────────┴──┐   Feign  ┌─┴──────────────────────┐
│  server (HelloServer)  │◄─────────│  client (HelloClient)  │
│  :7111                 │  GET /   │  :7211                 │
└────────────────────────┘          └────────────────────────┘
```

| Module          | Spring app name | Port | Description                                                                 |
|-----------------|-----------------|------|-----------------------------------------------------------------------------|
| `eureka-server` | `eureka`        | 8761 | Eureka service registry                                                     |
| `server`        | `HelloServer`   | 7111 | Registers to Eureka, `GET /` returns `Hello World: <service>:<host>:<port>` |
| `client`        | `HelloClient`   | 7211 | Calls `HelloServer` through a `@FeignClient` resolved via Eureka            |

## Tech Stack

- Java 25 (Temurin)
- Spring Boot 4.1.2
- Spring Cloud 2025.1.4 (OpenFeign, Netflix Eureka Server/Client, LoadBalancer)
- Maven multi-module build
- GitHub Actions CI + Dependabot

## Requirements

- JDK 25
- Maven 3.9+

## Build

```bash
mvn clean verify
```

## Run

Start each application in its own terminal, in this order:

```bash
# 1. Eureka registry
mvn -pl eureka-server spring-boot:run

# 2. Hello server (start more instances on other ports to see load balancing)
mvn -pl server spring-boot:run
mvn -pl server spring-boot:run -Dspring-boot.run.arguments=--server.port=7112

# 3. Hello client
mvn -pl client spring-boot:run
```

Then open:

- Eureka dashboard: http://localhost:8761
- Server directly: http://localhost:7111
- Client (via Feign + Eureka): http://localhost:7211

```bash
curl http://localhost:7211
# Hello World: HELLOSERVER:<host>:7111
```

## CI/CD

- **GitHub Actions** (`.github/workflows/maven.yml`): builds and tests all modules with JDK 25 on every push and pull
  request to `master`, then submits the Maven dependency graph on pushes to `master`.
- **Dependabot** (`.github/dependabot.yml`): daily checks for Maven and GitHub Actions updates, grouped into
  `spring-boot`, `spring-cloud`, `maven-plugins`, `others` and `github-actions` PRs with conventional commit prefixes.

## Author

**Hendi Santika**

- Email: hendisantika@gmail.com
- Telegram: [@hendisantika34](https://t.me/hendisantika34)
- Link: [s.id/hendisantika](https://s.id/hendisantika)
