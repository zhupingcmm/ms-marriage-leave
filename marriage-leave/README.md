# MarriageLeave 微服务

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://www.oracle.com/java/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.2.0-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-Latest-blue.svg)](https://www.postgresql.org/)
[![Maven](https://img.shields.io/badge/Maven-3.6+-red.svg)](https://maven.apache.org/)

基于Spring Boot 3.x的企业级婚假管理微服务，提供完整的婚假申请、审批和管理功能。

## 📋 目录

- [功能特性](#功能特性)
- [技术栈](#技术栈)
- [快速开始](#快速开始)
- [API文档](#api文档)
- [配置说明](#配置说明)
- [部署指南](#部署指南)
- [开发指南](#开发指南)
- [贡献指南](#贡献指南)

## ✨ 功能特性

- 🏢 **企业级架构**: 基于Spring Boot 3.x微服务架构
- 💾 **数据持久化**: 使用PostgreSQL数据库
- 🔍 **健康监控**: 集成Spring Boot Actuator
- 📝 **详细日志**: 支持SQL日志和应用日志
- 🧪 **测试覆盖**: 完整的单元测试和集成测试
- 🔧 **配置管理**: 灵活的YAML配置文件
- 📊 **监控指标**: 内置健康检查和性能监控

## 🛠 技术栈

| 技术 | 版本 | 说明 |
|------|------|------|
| Java | 17+ | 编程语言 |
| Spring Boot | 3.2.0 | 应用框架 |
| Spring Data JPA | 3.2.0 | 数据访问层 |
| PostgreSQL | Latest | 关系型数据库 |
| Maven | 3.6+ | 构建工具 |
| JUnit 5 | Latest | 测试框架 |

## 🚀 快速开始

### 环境要求

- ☕ Java 17 或更高版本
- 📦 Maven 3.6+
- 🐘 PostgreSQL 数据库
- 🔧 Git (可选)

### 安装步骤

1. **克隆项目**
```bash
git clone <repository-url>
cd ms-marriage-leave/marriage-leave
```

2. **数据库设置**
```sql
-- 创建数据库
CREATE DATABASE marriageleavedb;

-- 创建用户（可选）
CREATE USER marriageleave_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE marriageleavedb TO marriageleave_user;
```

3. **配置应用**
编辑 `src/main/resources/application.yml`：
```yaml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/marriageleavedb
    username: postgres
    password: your_password
```

4. **构建并运行**
```bash
# 构建项目
mvn clean compile

# 运行应用
mvn spring-boot:run
```

5. **验证部署**
访问健康检查端点：
```bash
curl http://localhost:8081/marriage-leave/api/v1/health
```

## 📚 API文档

### 基础端点

| 方法 | 端点 | 描述 | 响应 |
|------|------|------|------|
| GET | `/marriage-leave/api/v1/health` | 服务健康检查 | `{"status":"UP","service":"marriage-leave"}` |

### 监控端点

| 端点 | 描述 |
|------|------|
| `/marriage-leave/actuator/health` | 详细健康状态 |
| `/marriage-leave/actuator/info` | 应用信息 |
| `/marriage-leave/actuator/metrics` | 性能指标 |

### 示例请求

```bash
# 健康检查
curl -X GET http://localhost:8081/marriage-leave/api/v1/health

# 响应示例
{
  "status": "UP",
  "service": "marriage-leave"
}
```

## ⚙️ 配置说明

### 应用配置

```yaml
server:
  port: 8081                    # 服务端口
  servlet:
    context-path: /marriage-leave  # 应用上下文路径

spring:
  application:
    name: marriage-leave        # 应用名称
  datasource:
    url: jdbc:postgresql://localhost:5432/marriageleavedb
    driver-class-name: org.postgresql.Driver
    username: postgres
    password: postgres
  jpa:
    hibernate:
      ddl-auto: update          # 数据库表自动更新
    show-sql: true              # 显示SQL语句
```

### 环境变量

| 变量名 | 默认值 | 描述 |
|--------|--------|------|
| `SERVER_PORT` | 8081 | 服务端口 |
| `DB_URL` | localhost:5432/marriageleavedb | 数据库连接 |
| `DB_USERNAME` | postgres | 数据库用户名 |
| `DB_PASSWORD` | postgres | 数据库密码 |

## 🏗 项目结构

```
marriage-leave/
├── src/
│   ├── main/
│   │   ├── java/com/example/marriageleave/
│   │   │   ├── controller/          # REST控制器
│   │   │   │   └── MarriageLeaveController.java
│   │   │   ├── service/             # 业务逻辑层
│   │   │   ├── repository/          # 数据访问层
│   │   │   ├── model/               # 数据模型
│   │   │   ├── dto/                 # 数据传输对象
│   │   │   ├── config/              # 配置类
│   │   │   └── MarriageLeaveApplication.java
│   │   └── resources/
│   │       ├── application.yml      # 应用配置
│   │       └── application-prod.yml # 生产环境配置
│   └── test/
│       └── java/com/example/marriageleave/
│           ├── controller/          # 控制器测试
│           ├── service/             # 服务测试
│           └── MarriageLeaveApplicationTests.java
├── .gitignore                       # Git忽略文件
├── pom.xml                          # Maven配置
└── README.md                        # 项目文档
```

## 🚀 部署指南

### 本地开发

```bash
# 开发模式运行
mvn spring-boot:run -Dspring-boot.run.profiles=dev

# 调试模式运行
mvn spring-boot:run -Dspring-boot.run.jvmArguments="-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005"
```

### 生产部署

```bash
# 构建JAR包
mvn clean package -Dmaven.test.skip=true

# 运行JAR包
java -jar target/marriage-leave-0.0.1-SNAPSHOT.jar --spring.profiles.active=prod
```

### Docker部署

```dockerfile
FROM openjdk:17-jdk-slim
COPY target/marriage-leave-0.0.1-SNAPSHOT.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java","-jar","/app.jar"]
```

## 🧪 测试

### 运行测试

```bash
# 运行所有测试
mvn test

# 运行特定测试类
mvn test -Dtest=MarriageLeaveControllerTest

# 生成测试报告
mvn test jacoco:report
```

### 测试覆盖率

测试报告位置: `target/site/jacoco/index.html`

## 🔧 开发指南

### 代码规范

- 使用Java 17特性
- 遵循Spring Boot最佳实践
- 使用RESTful API设计原则
- 编写单元测试和集成测试

### 提交规范

```
feat: 添加新功能
fix: 修复bug
docs: 更新文档
style: 代码格式调整
refactor: 代码重构
test: 添加测试
chore: 构建过程或辅助工具的变动
```

## 🤝 贡献指南

1. Fork 项目
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 打开 Pull Request

## 📝 更新日志

### v0.0.1 (2025-08-16)
- ✨ 初始项目搭建
- 🏗️ 基础微服务架构
- 💾 PostgreSQL数据库集成
- 🔍 健康检查端点
- 📊 Actuator监控集成

## 📄 许可证

本项目采用 [MIT License](LICENSE) 许可证。

## 📞 联系方式

- 项目维护者: [Your Name]
- 邮箱: [your.email@example.com]
- 项目地址: [GitHub Repository URL]

---

⭐ 如果这个项目对你有帮助，请给它一个星标！
