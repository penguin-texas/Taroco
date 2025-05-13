## Introduction

## Project Overview

A microservices development framework system based on Spring Boot, Spring Cloud, and Axon Framework, following Domain-Driven Design principles. Provides a complete set of microservices architecture modules including:
- Unified User Center
- User authentication and authorization
- Configuration Center
- Service Registry
- Service Routing
- Circuit Breaking
- Tracing and Governance
- Canary Releases

Future plans include adding storage system and WeChat integration modules.

## Project Architecture

```lua
Taroco
├── taroco-assembling -- Project aggregation module
├── taroco-cloud -- Spring Cloud microservices components
|    ├── cloud-admin -- Service monitoring
|    ├── cloud-api-gateway -- API Gateway
|    ├── cloud-circuit-breaker -- Circuit breaking protection
|    ├── cloud-config -- Distributed configuration center
|    ├── cloud-registry -- Service registry
├── taroco-common -- Common components
├── taroco-user-center -- User Center
|    ├── uc-auth -- Authentication system
|    ├── uc-command -- Command side
|    ├── uc-common -- Shared components
|    ├── uc-query -- Query side
```
## Deployment Guide
### 1. taroco-assembling Project
The taroco-assembling module serves as the project's aggregation module, responsible for integrating all submodules into a unified build and management system.
 1. Environment Preparation
Before deployment, ensure the following prerequisites are met:

- JDK 8+ : Java Development Kit 1.8 or later
  - JDK Download
- Maven 3.5+ : Build automation tool
  - Maven Download
- Git : Version control system
  - Git Download 
  2. Clone Repository
1.Clone the project repository:
git clone https://github.com/penguin-texas/Taroco.git
Navigate to project root:
cd Taroco
Build Project
1. Build all modules (skipping tests):
mvn clean install -DskipTests
- Note: The -DskipTests parameter bypasses test execution. Remove it to run tests.
- The built JAR file will be located at:
taroco-assembling/target/taroco-assembling-0.0.1-SNAPSHOT.jar
4. Configuration
Ensure proper configuration before running. Example application.yml:
server:
  port: 8080

spring:
  application:
    name: taroco-assembling

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/
  instance:
    prefer-ip-address: true

logging:
  level:
    root: INFO
    Tip: Modify configurations like Eureka registry URL as needed.
 5. Start Service
1. Launch the service:
java -jar taroco-assembling/target/taroco-assembling-0.0.1-SNAPSHOT.jar
Verify service status at:
http://localhost:8080
6. Production Deployment
For production environments, consider:

- OS : Linux (CentOS/Ubuntu recommended)
- Process Management : Use systemd/supervisor/pm2
- Reverse Proxy : Configure Nginx for HTTPS and load balancing
- Log Management : Set proper log paths and rotation policies 7. Notes
- Ensure all dependent services (Eureka, Config Server etc.) are running
- Modify or add modules as per project requirements
### 2. taroco-cloud Project
Component Core Functionality Key Technology cloud-api-gateway Unified API entry, routing, auth control Zuul/Gateway cloud-circuit-breaker Circuit breaking dashboard Hystrix Dashboard cloud-config-server Centralized configuration management Spring Cloud Config cloud-registry-server Service discovery registry Eureka Server
graph TD
    A[Config Center] --> B[Registry]
    B --> C[Circuit Breaker]
    B --> D[API Gateway]
    Detailed Deployment
1. Base Environment
   
   - JDK 8+
   - Maven 3.5+
   - Git
2. Recommended Specs
   
   - Memory: ≥4GB
   - Disk: ≥20GB free space
   - Network: Inter-component connectivity Config Center Deployment
1. Config Server application.yml:
server:
  port: 8888
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/your-org/config-repo
          search-paths: '{application}'
          Start command:
          java -jar cloud-config-server-0.0.1-SNAPSHOT.jar
          Registry Server application.yml:
          server:
  port: 8761
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
    Start command:
    java -jar cloud-registry-server-0.0.1-SNAPSHOT.jar
    Circuit Breaker application.yml:
    server:
  port: 7979
management:
  endpoints:
    web:
      exposure:
        include: hystrix.stream
        Start command:
        java -jar cloud-circuit-breaker-0.0.1-SNAPSHOT.jar
        API Gateway application.yml:
        server:
  port: 8080
zuul:
  routes:
    user-service:
      path: /user/**
      serviceId: user-service
      Start command:
      java -jar cloud-api-gateway-0.0.1-SNAPSHOT.jar
      Verification
- Registry : http://localhost:8761 (Eureka console)
- Config Center : http://localhost:8888/application/default (JSON config)
- Circuit Breaker : http://localhost:7979/hystrix (Dashboard)
- API Gateway : http://localhost:8080/your-service (Expect: {"status":"UP"})
<!-- by zlg -->

