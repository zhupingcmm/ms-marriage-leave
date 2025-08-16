# MarriageLeave 微服务

基于Spring Boot 3.x的婚假管理微服务，使用PostgreSQL数据库。

## 环境要求

- Java 17 或更高版本
- Maven 3.6+
- PostgreSQL 数据库

## 数据库设置

在运行应用之前，请确保PostgreSQL数据库已安装并运行：

1. 创建数据库：
```sql
CREATE DATABASE marriageleavedb;
```

2. 确保PostgreSQL服务运行在默认端口5432

## 运行应用

1. 进入marriage-leave目录：
```bash
cd marriage-leave
```

2. 运行应用：
```bash
mvn spring-boot:run
```

应用将在端口8081启动，上下文路径为`/marriage-leave`。

## 接口端点

- **健康检查**: `GET /marriage-leave/api/v1/health`
- **Actuator健康检查**: `GET /marriage-leave/actuator/health`
- **其他监控端点**: `GET /marriage-leave/actuator/info`, `GET /marriage-leave/actuator/metrics`

## 配置说明

应用使用`application.yml`进行配置，主要设置包括：
- 服务端口：8081
- 上下文路径：`/marriage-leave`
- PostgreSQL数据库连接
- Actuator监控端点
- 应用包的DEBUG日志级别
- Hibernate SQL日志

## 项目结构

```
marriage-leave/
├── src/
│   ├── main/
│   │   ├── java/com/example/marriageleave/
│   │   │   ├── controller/
│   │   │   │   └── MarriageLeaveController.java
│   │   │   └── MarriageLeaveApplication.java
│   │   └── resources/
│   │       └── application.yml
│   └── test/
│       └── java/com/example/marriageleave/
│           └── MarriageLeaveApplicationTests.java
├── pom.xml
└── README.md
```

## 构建应用

构建应用：
```bash
mvn clean package
```

## 运行测试

运行测试：
```bash
mvn test
```

## 数据库配置

默认数据库配置：
- 数据库：marriageleavedb
- 用户名：postgres
- 密码：postgres
- 主机：localhost:5432

可以通过修改`application.yml`文件来更改数据库配置。
