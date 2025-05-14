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

### 3.5 Verify the Service
1. Check if the service is running properly:
    ```bash
    curl http://localhost:8080/actuator/health
    ```
    A return value of `{"status":"UP"}` indicates that the service is functioning correctly.
2. Access the front-end page:
    - If the front-end service is deployed, open a browser and visit `http://<Server IP>:<Front-end Port>`.

---

## 4. Troubleshooting Common Issues

### 4.1 Database Connection Failure
- Confirm whether the MySQL service is running.
- Check if the database address, port, username, and password are correct.

### 4.2 Service Startup Failure
- Check the error messages in the logs within the `logs/` folder.
- Ensure that all dependent services (such as Redis and MySQL) are running.

### 4.3 Inability to Access the Front-end Page
- Ensure that the front-end build was successful.
- Confirm that the API address configurations of the front-end and back-end are consistent.

---

## 5. Additional Content

- **Extended Modules**: Storage systems, WeChat systems, etc., can be added later based on requirements.
- **Performance Optimization**: Configure load balancing and caching strategies to improve system response speed.

---





### 4. taroco-user Project
---
**Component Name** | **Core Function** | **Key Technologies**
---|---|---
taroco-user | User service module that provides functions such as user management, authentication and authorization, and permission control | Spring Boot, Spring Cloud, MyBatis-Plus

**Deployment Sequence and Dependencies**  
1. Depends on the taroco-root parent project for basic configurations.
2. Needs to connect to an already deployed database service.
3. Needs to register with a service registry (such as Eureka or Nacos).
4. It is recommended to use in conjunction with an API gateway (such as cloud-api-gateway).


### 2. Detailed Deployment Plan

#### **1. Basic Environment**
- JDK 8+ environment
- Maven 3.5+
- Git (for pulling project code)
- Database (such as MySQL 5.7+ or PostgreSQL)


#### **2. Recommended Configuration**
- Memory: ≥2GB
- Disk: ≥10GB of available space
- Network: Must be able to communicate with the registry, configuration center, and database


#### **3. Service Deployment**

##### **1. Configure the Database**
Create a database and execute the initialization script (usually located in the src/main/resources/sql directory of the project):
```sql
CREATE DATABASE taroco_user CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```


##### **2. Deploy the User Service (taroco-user)**
**Configuration File application.yml**:
```yaml
server:
  port: 9001  # Service port

spring:
  application:
    name: taroco-user  # Application name
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/taroco_user?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=UTC
    username: root
    password: your_password
  cloud:
    nacos:  # If using Nacos as the registry
      discovery:
        server-addr: localhost:8848
    # If using Eureka as the registry
    # eureka:
    #   client:
    #     service-url:
    #       defaultZone: http://localhost:8761/eureka/

mybatis-plus:
  mapper-locations: classpath:mapper/**/*.xml
  type-aliases-package: xyz.weechang.taroco.user.entity
  configuration:
    map-underscore-to-camel-case: true
```

**Startup Command**:
```bash
java -jar taroco-user-0.0.1-SNAPSHOT.jar
```


#### **4. Verify Successful Deployment**
1. **Verify the Registry**  
   Access the registry address (such as Eureka: http://localhost:8761 or Nacos: http://localhost:8848/nacos)  
   Expectation: The taroco-user service should be listed as registered.

2. **Verify the User Service**  
   Invoke the health check interface:
   ```bash
   curl http://localhost:9001/actuator/health
   ```
   Expected return:
   ```json
   {"status":"UP"}
   ```

3. **Verify the API Gateway (if integrated)**  
   Access the user service through the gateway:
   ```bash
   curl http://localhost:8080/user/actuator/health
   ```
   Expected return:
   ```json
   {"status":"UP"}
   ```


### 3. Common Issues

1. **Database Connection Failure**  
   - Check if the database address, username, and password are correct.
   - Ensure that the database service is running and accessible.

2. **Service Not Registered with the Registry**  
   - Check if the registry address configuration is correct.
   - Confirm that the registry service is running properly.

3. **Dependency Conflicts**  
   - Use `mvn dependency:tree` to view the dependency tree and exclude conflicting dependencies.
   - Ensure that the version of the parent project taroco-root is compatible.


### 4. Extended Configuration

**Enable Monitoring Endpoints**:
```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"  # Expose all monitoring endpoints
  endpoint:
    health:
      show-details: always  # Show detailed health check information
```

**Configure Logging**:
```yaml
logging:
  level:
    root: INFO
    xyz.weechang: DEBUG  # Project package path, adjust the logging level
  file:
    name: logs/taroco-user.log  # Log file location
```


### Technology Stack

#### Back-end Technologies:
Technology | Name | Official Website
----|------|----
Spring Boot | Container | [http://projects.spring.io/spring-boot/](http://projects.spring.io/spring-boot/)
Spring Cloud | Container | [http://projects.spring.io/spring-cloud/](http://projects.spring.io/spring-cloud/)
Axon | Container | [http://www.axonframework.org/](http://www.axonframework.org/)
Rabbit MQ | Container | [http://www.rabbitmq.com/](http://www.rabbitmq.com/)
Spring Data | Container | [http://projects.spring.io/spring-data/](http://projects.spring.io/spring-data/)
Swagger2 | Interface Testing Framework | [http://swagger.io/](http://swagger.io/)
Jenkins | Continuous Integration Tool | [https://jenkins.io/index.html](https://jenkins.io/index.html)
Maven | Project Build Management | [http://maven.apache.org/](http://maven.apache.org/)

#### Front-end Technologies:
Technology | Name | Official Website
----|------|----
VUE | Library | [https://cn.vuejs.org/](https://cn.vuejs.org/)
vue-router 2 | VUE Router | [https://router.vuejs.org/zh-cn/](https://router.vuejs.org/zh-cn/)
iview | VUE Components | [https://www.iviewui.com](https://www.iviewui.com)

#### Development Tools:
- MongoDB: Database
- Tomcat: Application Server
- Git: Version Control
- Nginx: Reverse Proxy Server
- IntelliJ IDEA: Development IDE

#### Development Environment:
- JDK8+
- MongoDB

## License

[MIT](LICENSE "MIT")
<!-- by zzh -->
