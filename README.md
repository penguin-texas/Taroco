# Taroco Microservices System

## Preface

This project is part of a course design, aiming to demonstrate the design and implementation of a distributed microservices system based on Spring Cloud. The project documentation, code structure, and technical details were collaboratively completed by team members.



## Project Introduction

Taroco is a comprehensive microservices system covering user management, system configuration, service governance and other modules. The system adopts modern microservices technologies including Spring Cloud, Axon Framework, and RabbitMQ, featuring excellent modularity, scalability and fault tolerance.



## System Architecture

```
Taroco
├── taroco-assembling -- Project aggregation module
├── taroco-cloud -- Spring Cloud microservices components
|    ├── cloud-admin -- Service monitoring
|    ├── cloud-api-gateway -- API gateway
|    ├── cloud-circuit-breaker -- Circuit breaker
|    ├── cloud-config -- Distributed configuration center
|    ├── cloud-registry -- Service registry
├── taroco-common -- Common components
├── taroco-user-center -- User center module
|    ├── uc-auth -- Authentication system
|    ├── uc-command -- Command side
|    ├── uc-common -- Common components
|    ├── uc-query -- Query side
```



## Technology Stack

### Backend Technologies

| Technology | Description | Official Site |
|------------|-------------|---------------|
| Spring Boot | Container framework | [Spring Boot](https://spring.io/projects/spring-boot) |
| Spring Cloud | Microservices framework | [Spring Cloud](https://spring.io/projects/spring-cloud) |
| Axon | CQRS/Event-driven framework | [Axon](https://axoniq.io/) |
| RabbitMQ | Message queue | [RabbitMQ](https://www.rabbitmq.com/) |
| Spring Data | Data access | [Spring Data](https://spring.io/projects/spring-data) |
| Swagger2 | API documentation | [Swagger](https://swagger.io/) |
| Jenkins | CI/CD tool | [Jenkins](https://www.jenkins.io/) |
| Maven | Build tool | [Maven](https://maven.apache.org/) |

### Frontend Technologies

| Technology | Description | Official Site |
|------------|-------------|---------------|
| Vue.js | JavaScript framework | [Vue.js](https://vuejs.org/) |
| vue-router 2 | Routing library | [vue-router](https://router.vuejs.org/) |
| iView | UI component library | [iView](https://www.iviewui.com/) |

### Development Tools

- MongoDB: Document database
- Tomcat: Web application server
- Git: Version control
- Nginx: Reverse proxy server
- IntelliJ IDEA: Java IDE



## Development Environment

- JDK 8 or later
- MongoDB database

## License

MIT License

<!--by A赵李观-->