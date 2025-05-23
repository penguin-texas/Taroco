

## 前言


## 项目介绍

基于spring boot，spring cloud，axon framework的领域驱动设计的微服务开发框架系统。提供整套微服务架构模块：统一用户中心，用户授权、鉴权，配置中心，注册中心，服务路由，服务熔断，追踪、治理，灰度发布。后期将继续加上存储系统、微信系统。

## 项目架构

``` lua
Taroco
├── taroco-assembling -- 项目集合工程
├── taroco-cloud -- spring cloud 微服务相关基础组件
|    ├── cloud-admin -- 服务监控
|    ├── cloud-api-gateway -- 服务网关
|    ├── cloud-circuit-breaker -- 服务容错保护
|    ├── cloud-config -- 分布式配置中心
|    ├── cloud-registry -- 服务注册中心
├── taroco-common -- 项目基础服务组件
├── taroco-user-center -- 用户中心
|    ├── uc-auth -- 用户授权系统
|    ├── uc-command -- 用户命令端
|    ├── uc-common -- 用户公用
|    ├── uc-query -- 用户查询端
```
## 部署说明


根据 Taroco 项目架构中的描述和常见的 Spring Boot/Spring Cloud 项目部署流程，以下是部署说明：

---
<!--by ljq -->
## 1.taroco-assembling项目
--
taroco-assembling 是 Taroco 项目的聚合模块。它主要负责将各个子模块集成到一个整体中，用于统一构建和管理。

#### 1. 环境准备

在部署之前，请确保以下环境已经准备好：

- **JDK 8+**：确保安装了 Java JDK 1.8 或更高版本。
  - [JDK 下载链接](https://www.oracle.com/java/technologies/javase-jdk8-downloads.html)
- **Maven 3.5+**：确保安装了 Maven 构建工具。
  - [Maven 下载链接](https://maven.apache.org/download.cgi)
- **Git**：用于拉取代码仓库。
  - [Git 下载链接](https://git-scm.com/downloads)

#### 2. 拉取代码

1. 使用 Git 克隆项目代码到本地：
   ```bash
   git clone https://github.com/penguin-texas/Taroco.git
   ```
2. 进入项目的根目录：
   ```bash
   cd Taroco
   ```

#### 3. 构建项目

1. 使用 Maven 构建项目，生成所有模块的 .jar 文件：
   ```bash
   mvn clean install -DskipTests
   ```
   > **注意**：-DskipTests 参数会跳过测试用例的执行。如果需要运行测试，请移除该参数。

2. 构建完成后，taroco-assembling 的输出文件将位于 target 文件夹中，例如：
   ```
   taroco-assembling/target/taroco-assembling-0.0.1-SNAPSHOT.jar
   ```

#### 4. 配置文件准备

在运行 taroco-assembling 模块之前，请确保配置文件已正确设置。以下是一个示例 application.yml 配置文件：

```yaml
server:
  port: 8080

spring:
  application:
    name: taroco-assembling

eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka/ # 注册中心地址
  instance:
    prefer-ip-address: true

logging:
  level:
    root: INFO
```

> **提示**：根据实际情况修改配置文件，例如 Eureka`注册中心地址等。

#### 5. 启动服务

1. 使用以下命令启动 taroco-assembling 模块：
   ```bash
   java -jar taroco-assembling/target/taroco-assembling-0.0.1-SNAPSHOT.jar
   ```

2. 服务启动后，您可以在浏览器中访问 http://localhost:8080 ，以确认服务是否正常运行。

#### 6. 部署到生产环境

如果需要将服务部署到生产环境，请考虑以下事项：

- **操作系统**：推荐使用 Linux 系统（如 CentOS 或 Ubuntu）。
- **进程管理**：使用 systemd、supervisor 或 pm2 等工具管理服务进程。
- **反向代理**：使用 Nginx 配置反向代理以支持 HTTPS 和负载均衡。
- **日志管理**：配置日志文件的存储路径和日志轮转策略。

#### 7. 其他注意事项

- 确保所有依赖的服务（例如 Eureka 注册中心、Config Server 配置中心等）都已正确启动。
- 如需扩展功能，可根据项目需求修改或新增模块。
---
<!--by ljq -->



<!--by lqk -->
## 2.taroco-cloud项目
--

组件名称|核心功能|关键技术
--|:--:|--:
cloud-api-gateway|统一API入口，提供路由转发，权限控制，请求过滤等能力  |Zuul/Gateway
cloud-circuit-breaber|熔断监控仪表盘，实时可视化健康状态|Hystrix Dashboard
cloud-config-server|集中化管理服务配置，支持多环境隔离部署|Spring Cloud Config Server
cloud-registry-server|服务注册与发现中心，管理微服务实例元数据|Eureka Server

### 部署顺序与依赖关系
```mermaid
graph TD
    A[配置中心] --> B[注册中心]
    B --> C[熔断监控]
    B --> D[API网关]
```
### 详细部署方案
1.基础环境
+ [JDK 8+环境](https://www.oracle.com/cn/java/technologies/downloads/) 

+ [Maven 3.5+](https://maven.apache.org/download.cgi)

+ [Git(用于配置仓库)](https://git-scm.com/downloads)

2.推荐配置
+ 内存：≥4GB

+ 磁盘：≥20GB可用空间

+ 网络：各组件间需互通

### 配置中心部署

#### 1.配置中心部署(cloud-config-server)
配置文件application.yml :
```
server:
  port: 8888
spring:
  cloud:
    config:
      server:
        git:
          uri: https://github.com/your-org/config-repo
          search-paths: '{application}'
```
##### 启动命令:
`java -jar cloud-config-server-0.0.1-SNAPSHOT.jar`

#### 2.注册中心部署(cloud-registry-server)
配置文件application.yml :
```
server:
  port: 8761
eureka:
  instance:
    hostname: localhost
  client:
    register-with-eureka: false
    fetch-registry: false
```
##### 启动命令:
`java -jar cloud-registry-server-0.0.1-SNAPSHOT.jar`

#### 3.熔断监控部署(cloud-circuit-breaker)
配置文件application.yml :
```
server:
  port: 7979
management:
  endpoints:
    web:
      exposure:
        include: hystrix.stream
```
##### 启动命令:
`java -jar cloud-circuit-breaker-0.0.1-SNAPSHOT.jar`

#### 4.API网关部署(cloud-api-gateway)
配置文件application.yml :
```
server:
  port: 8080
zuul:
  routes:
    user-service:
      path: /user/**
      serviceId: user-service
```
##### 启动命令:
`java -jar cloud-api-gateway-0.0.1-SNAPSHOT.jar`

#### 验证部署成功
+ 注册中心
   + 访问:http://localhost:8761 
   + 预期:显示Eureka控制台,无注册服务
+ 配置中心
   + 访问:http://localhost:8888/application/default 
   + 预期:返回JSON格式的配置信息
+ 熔断监控
   + 访问:http://localhost:7979/hystrix
   + 预期:显示Hystrix Dashboard页面
+ API网关
   + 访问:http://localhost:8080/your-service
   + 预期:返回{"status":"UP"}
<!-- by lqk -->



---
<!-- by tfj -->
## 3.taroco-common项目
--
## 1. 环境准备

在开始部署前，请确保以下环境和工具已安装并正确配置：

### 1.1 基础环境
- **操作系统**: Linux/Unix 推荐 (如 Ubuntu 20.04 或 CentOS 7+)
- **JDK**: 安装 JDK 8 或更高版本
- **Maven**: 确保安装了 Maven 3.6 或更高版本
- **Node.js 和 npm**: 如果前端采用 JavaScript，确保安装了 Node.js (建议 16.x 版本) 和 npm (建议 8.x 版本)
- **Docker**: 用于容器化部署 (可选)
- **MySQL**: 数据库服务器 (建议 5.7 或更高版本)
- **Redis**: 缓存服务 (可选)

### 1.2 网络配置
- 确保服务器能够访问外部网络，用于下载依赖包和镜像。
- 配置防火墙规则，开放必要的端口（如 8080, 3306, 6379）。

---

## 2. 项目结构

本项目采用领域驱动设计 (DDD) 的微服务架构，以下是主要模块简介：
- **用户中心**: 提供统一用户管理、授权和鉴权服务。
- **配置中心**: 管理服务配置文件。
- **注册中心**: 提供服务发现与注册功能。
- **服务路由**: 管理服务间的路由。
- **服务治理**: 包括熔断、灰度发布等功能。
- **追踪模块**: 提供分布式链路追踪功能。

---

## 3. 部署步骤

### 3.1 下载代码
```bash
# 克隆代码仓库
git clone https://github.com/tfj-bot/Taroco.git
cd Taroco
```

---

### 3.2 配置后端服务

#### 3.2.1 数据库初始化
1. 创建 MySQL 数据库：
    ```sql
    CREATE DATABASE taroco CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
    ```
2. 导入初始 SQL 脚本：
    ```bash
    mysql -u root -p taroco < scripts/init-database.sql
    ```

#### 3.2.2 修改配置文件
- 找到 `application.yml` 或 `application.properties` 文件，更新以下配置项：
    ```yaml
    spring:
      datasource:
        url: jdbc:mysql://<数据库地址>:3306/taroco?useUnicode=true&characterEncoding=utf8&useSSL=false
        username: <数据库用户名>
        password: <数据库密码>
      redis:
        host: <Redis 地址>
        port: 6379
    ```

#### 3.2.3 构建后端服务
```bash
# 使用 Maven 构建
mvn clean package -DskipTests
```

- 构建完成后，所有服务的可执行 `jar` 文件将存储在 `target/` 目录下。

---

### 3.3 配置前端服务

如果项目包含前端模块：
1. 切换到前端目录，例如 `web/` 或 `frontend/`。
2. 安装依赖：
    ```bash
    npm install
    ```
3. 构建前端代码：
    ```bash
    npm run build
    ```
4. 构建完成的静态文件通常位于 `dist/` 或 `build/` 目录下。

---

### 3.4 启动服务

#### 3.4.1 单机启动
可以直接运行 `jar` 文件来启动服务：
```bash
java -jar target/{module-name}.jar
```

#### 3.4.2 使用 Docker 部署
1. 确保安装 Docker 和 Docker Compose。
2. 修改 `docker-compose.yml` 文件，更新相关配置。
3. 启动服务：
    ```bash
    docker-compose up -d
    ```

---

### 3.5 验证服务
1. 检查服务是否正常运行：
    ```bash
    curl http://localhost:8080/actuator/health
    ```
    返回 `{"status":"UP"}` 表示服务正常。
2. 访问前端页面：
    - 如果部署了前端服务，打开浏览器访问 `http://<服务器IP>:<前端端口>`。

---

## 4. 常见问题排查

### 4.1 数据库连接失败
- 确认 MySQL 服务是否启动。
- 检查数据库地址、端口、用户名和密码是否正确。

### 4.2 服务启动失败
- 检查 `logs/` 文件夹中的日志错误信息。
- 确保所有依赖服务已启动（如 Redis、MySQL）。

### 4.3 前端页面无法访问
- 确保前端构建成功。
- 确认前端与后端的 API 地址配置一致。

---

## 5. 其他内容

- **扩展模块**: 后期可以根据需求加入存储系统、微信系统等。
- **性能优化**: 配置负载均衡和缓存策略以提高系统响应速度。

---

<!-- by tfj -->




<!-- by wzy -->
### 4. taroco-user 项目
---
**组件名称** | **核心功能** | **关键技术**
---|---|---
taroco-user | 用户服务模块，提供用户管理、认证授权、权限控制等功能 | Spring Boot、Spring Cloud、MyBatis-Plus

**部署顺序与依赖关系**  
1. 依赖于 taroco-root 父项目提供基础配置
2. 需连接已部署的数据库服务
3. 需注册到服务注册中心（如 Eureka 或 Nacos）
4. 建议配合 API 网关（如 cloud-api-gateway）使用


### 2. 详细部署方案

#### **1. 基础环境**
- JDK 8+ 环境
- Maven 3.5+
- Git（用于拉取项目代码）
- 数据库（如 MySQL 5.7+ 或 PostgreSQL）


#### **2. 推荐配置**
- 内存：≥2GB
- 磁盘：≥10GB 可用空间
- 网络：需与注册中心、配置中心、数据库互通


#### **3. 服务部署**

##### **1. 配置数据库**
创建数据库并执行初始化脚本（通常位于项目的 src/main/resources/sql 目录）：
```sql
CREATE DATABASE taroco_user CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```


##### **2. 用户服务部署 (taroco-user)**
**配置文件 application.yml**：
```yaml
server:
  port: 9001  # 服务端口

spring:
  application:
    name: taroco-user  # 应用名称
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/taroco_user?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=UTC
    username: root
    password: your_password
  cloud:
    nacos:  # 若使用 Nacos 作为注册中心
      discovery:
        server-addr: localhost:8848
    # 若使用 Eureka 作为注册中心
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

**启动命令**：
```bash
java -jar taroco-user-0.0.1-SNAPSHOT.jar
```


#### **4. 验证部署成功**
1. **注册中心验证**  
   访问注册中心地址（如 Eureka：http://localhost:8761 或 Nacos：http://localhost:8848/nacos）  
   预期：在服务列表中看到 taroco-user 服务已注册。

2. **用户服务验证**  
   调用健康检查接口：
   ```bash
   curl http://localhost:9001/actuator/health
   ```
   预期返回：
   ```json
   {"status":"UP"}
   ```

3. **API 网关验证（若已集成）**  
   通过网关访问用户服务：
   ```bash
   curl http://localhost:8080/user/actuator/health
   ```
   预期返回：
   ```json
   {"status":"UP"}
   ```


### 3. 常见问题

1. **数据库连接失败**  
   - 检查数据库地址、用户名、密码是否正确
   - 确保数据库服务已启动且可访问

2. **服务未注册到注册中心**  
   - 检查注册中心地址配置是否正确
   - 确认注册中心服务已正常运行

3. **依赖冲突**  
   - 使用 mvn dependency:tree 查看依赖树，排除冲突依赖
   - 确保父项目 taroco-root 版本兼容


### 4. 扩展配置

**开启监控端点**：
```yaml
management:
  endpoints:
    web:
      exposure:
        include: "*"  # 暴露所有监控端点
  endpoint:
    health:
      show-details: always  # 显示健康检查详情
```

**配置日志**：
```yaml
logging:
  level:
    root: INFO
    xyz.weechang: DEBUG  # 项目包路径，调整日志级别
  file:
    name: logs/taroco-user.log  # 日志文件位置
```
<!-- by wzy -->



### 技术选型

#### 后端技术:
技术 | 名称 | 官网
----|------|----
Spring Boot | 容器  | [http://projects.spring.io/spring-boot/](http://projects.spring.io/spring-boot/)
Spring Cloud | 容器  | [http://projects.spring.io/spring-cloud/](http://projects.spring.io/spring-cloud/)
Axon | 容器  | [http://www.axonframework.org/](http://www.axonframework.org/)
Rabbit MQ | 容器  | [http://www.rabbitmq.com/](http://www.rabbitmq.com/)
Spring Data | 容器  | [http://projects.spring.io/spring-data/](http://projects.spring.io/spring-data/)
Swagger2 | 接口测试框架  | [http://swagger.io/](http://swagger.io/)
Jenkins | 持续集成工具  | [https://jenkins.io/index.html](https://jenkins.io/index.html)
Maven | 项目构建管理  | [http://maven.apache.org/](http://maven.apache.org/)

#### 前端技术:
技术 | 名称 | 官网
----|------|----
VUE | 函式库  | [https://cn.vuejs.org/](https://cn.vuejs.org/)
vue-router 2 | vue路由  | [https://router.vuejs.org/zh-cn/](https://router.vuejs.org/zh-cn/)
iview | VUE组件  | [https://www.iviewui.com](https://www.iviewui.com)

#### 开发工具:
- MongoDB: 数据库
- Tomcat: 应用服务器
- Git: 版本管理
- Nginx: 反向代理服务器
- IntelliJ IDEA: 开发IDE

#### 开发环境：
- JDK8+
- MongoDB

## 许可证

[MIT](LICENSE "MIT")
