# 术语词汇表（terms.md）

| 术语（中文）         | Term（English）           | 说明（Definition）                                                                 |
|----------------------|---------------------------|--------------------------------------------------------------------------------------|
| 微服务架构           | Microservices Architecture | 一种将应用程序拆分为多个小服务，每个服务运行在其独立进程中，彼此通过 API 通信的架构风格。      |
| Spring Boot          | Spring Boot               | 一个简化 Spring 应用开发的框架，提供自动配置、快速部署和最少的样板代码。                        |
| Spring Cloud         | Spring Cloud              | 一组用于构建分布式系统的工具集，包含配置管理、服务发现、断路器等。                           |
| 服务注册中心         | Service Registry          | 用于注册和发现微服务的组件，常用实现如 Eureka 或 Consul。                             |
| 服务网关             | API Gateway               | 请求的统一入口，负责请求路由、鉴权、限流等功能。                                      |
| 分布式配置中心       | Distributed Config Center | 提供集中式配置管理服务，可动态刷新配置，常用实现如 Spring Cloud Config。              |
| 断路器               | Circuit Breaker           | 一种容错机制，在调用失败时快速失败避免雪崩效应，常见实现如 Hystrix、Resilience4j。      |
| 消息队列             | Message Queue             | 用于服务间异步通信的中间件，如 RabbitMQ、Kafka。                                   |
| Swagger              | Swagger                   | API 文档生成工具，支持自动生成交互式 API 文档界面。                                  |
| Jenkins              | Jenkins                   | 持续集成和持续交付（CI/CD）工具，支持自动化构建、测试和部署。                          |
| Maven                | Maven                     | Java 项目的构建工具和依赖管理工具。                                                |
| Vue.js               | Vue.js                    | 渐进式 JavaScript 框架，适用于构建交互式网页用户界面。                               |
| iView                | iView                     | 基于 Vue 的高质量 UI 组件库，适用于中后台系统。                                     |
| Git                  | Git                       | 分布式版本控制系统，支持多人协作开发。                                              |
| Nginx                | Nginx                     | 高性能 Web 服务器和反向代理服务器，支持负载均衡等。                                  |
| CQRS                | Command Query Responsibility Segregation | 命令查询职责分离，是一种系统设计模式，将读取操作与写入操作分离。            |
| Axon Framework       | Axon Framework            | 一个支持 CQRS 和事件驱动架构的 Java 框架。                                          |
| MongoDB              | MongoDB                   | 一种面向文档的 NoSQL 数据库，适合存储灵活结构的数据。                               |
| REST API             | REST API                  | 基于 HTTP 协议的接口风格，资源通过 URL 表示，使用标准 HTTP 方法访问。               |
| CI/CD                | Continuous Integration / Continuous Deployment | 持续集成 / 持续部署，是现代软件交付流程的核心实践。               |
<!--by A赵李观-->