# Taroco 微服务系统

## 前言

本项目为课程设计的一部分，旨在展示一个基于 Spring Cloud 的分布式微服务系统的设计与实现。项目文档、代码结构和技术细节均由团队成员协作完成。


## 项目介绍

Taroco 是一个完整的微服务系统，涵盖用户管理、系统配置、服务治理等模块。系统采用 Spring Cloud、Axon 框架、RabbitMQ 等现代微服务技术，具有良好的模块化、可扩展性与容错能力。

## 项目架构

```
Taroco
├── taroco-assembling -- 项目集合工程
├── taroco-cloud -- Spring Cloud 微服务基础组件
|    ├── cloud-admin -- 服务监控
|    ├── cloud-api-gateway -- 服务网关
|    ├── cloud-circuit-breaker -- 服务容错保护
|    ├── cloud-config -- 分布式配置中心
|    ├── cloud-registry -- 服务注册中心
├── taroco-common -- 公共组件模块
├── taroco-user-center -- 用户中心模块
|    ├── uc-auth -- 用户授权系统
|    ├── uc-command -- 用户命令端
|    ├── uc-common -- 用户公共组件
|    ├── uc-query -- 用户查询端
```


## 技术选型

### 后端技术

| 技术 | 说明 | 官网链接 |
|------|------|----------|
| Spring Boot | 容器框架 | [Spring Boot](https://spring.io/projects/spring-boot) |
| Spring Cloud | 微服务框架 | [Spring Cloud](https://spring.io/projects/spring-cloud) |
| Axon | CQRS/事件驱动框架 | [Axon](https://axoniq.io/) |
| RabbitMQ | 消息队列 | [RabbitMQ](https://www.rabbitmq.com/) |
| Spring Data | 数据访问 | [Spring Data](https://spring.io/projects/spring-data) |
| Swagger2 | 接口文档生成工具 | [Swagger](https://swagger.io/) |
| Jenkins | 持续集成工具 | [Jenkins](https://www.jenkins.io/) |
| Maven | 构建工具 | [Maven](https://maven.apache.org/) |

### 前端技术

| 技术 | 说明 | 官网链接 |
|------|------|----------|
| Vue.js | JavaScript 框架 | [Vue.js](https://vuejs.org/) |
| vue-router 2 | 路由库 | [vue-router](https://router.vuejs.org/) |
| iView | UI 组件库 | [iView](https://www.iviewui.com/) |

### 开发工具

- MongoDB：文档型数据库
- Tomcat：Web 应用服务器
- Git：版本控制工具
- Nginx：反向代理服务器
- IntelliJ IDEA：Java 集成开发环境


## 开发环境

- JDK 8 或以上版本
- MongoDB 数据库

## 许可证

MIT License

<!--by A赵李观-->