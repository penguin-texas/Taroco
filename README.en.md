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

<!-- by zzh -->
## 3. taroco-common Project
--
## 1. Environment Preparation

Before starting the deployment, ensure that the following environments and tools are installed and correctly configured:

### 1.1 Basic Environment
- **Operating System**: Linux/Unix is recommended (e.g., Ubuntu 20.04 or CentOS 7+)
- **JDK**: Install JDK 8 or a higher version
- **Maven**: Ensure that Maven 3.6 or a higher version is installed
- **Node.js and npm**: If JavaScript is used for the front-end, ensure that Node.js (version 16.x is recommended) and npm (version 8.x is recommended) are installed
- **Docker**: For containerized deployment (optional)
- **MySQL**: Database server (version 5.7 or higher is recommended)
- **Redis**: Caching service (optional)

### 1.2 Network Configuration
- Ensure that the server can access the external network for downloading dependent packages and images.
- Configure firewall rules and open necessary ports (e.g., 8080, 3306, 6379).

---
## 2. Project Structure

This project adopts a microservices architecture based on Domain-Driven Design (DDD). The following is a brief introduction to the main modules:
- **User Center**: Provides unified user management, authorization, and authentication services.
- **Configuration Center**: Manages service configuration files.
- **Registry Center**: Provides service discovery and registration functions.
- **Service Routing**: Manages routing between services.
- **Service Governance**: Includes functions such as circuit breaking and gray release.
- **Tracing Module**: Provides distributed link tracing functions.

---
## 3. Deployment Steps

### 3.1 Download the Code
```bash
# Clone the code repository
git clone https://github.com/tfj-bot/Taroco.git
cd Taroco
```

---
### 3.2 Configure the Back-end Service

#### 3.2.1 Database Initialization
1. Create a MySQL database:
    ```sql
    CREATE DATABASE taroco CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
    ```
2. Import the initial SQL script:
    ```bash
    mysql -u root -p taroco < scripts/init-database.sql
    ```

#### 3.2.2 Modify the Configuration File
- Locate the `application.yml` or `application.properties` file and update the following configuration items:
    ```yaml
    spring:
      datasource:
        url: jdbc:mysql://<Database Address>:3306/taroco?useUnicode=true&characterEncoding=utf8&useSSL=false
        username: <Database Username>
        password: <Database Password>
      redis:
        host: <Redis Address>
        port: 6379
    ```

#### 3.2.3 Build the Back-end Service
```bash
# Build using Maven
mvn clean package -DskipTests
```
- After the build is complete, the executable `jar` files of all services will be stored in the `target/` directory.

---
### 3.3 Configure the Front-end Service

If the project includes a front-end module:
1. Switch to the front-end directory, such as `web/` or `frontend/`.
2. Install dependencies:
    ```bash
    npm install
    ```
3. Build the front-end code:
    ```bash
    npm run build
    ```
4. The built static files are usually located in the `dist/` or `build/` directory.

---
### 3.4 Start the Service

#### 3.4.1 Standalone Start
You can directly run the `jar` file to start the service:
```bash
java -jar target/{module-name}.jar
```

#### 3.4.2 Deployment Using Docker
1. Ensure that Docker and Docker Compose are installed.
2. Modify the `docker-compose.yml` file and update the relevant configurations.
3. Start the service:
    ```bash
    docker-compose up -d
    ``` 

