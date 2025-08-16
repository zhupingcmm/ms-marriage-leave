---
trigger: manual
---

# 命名规范
- 实体类以 `Entity` 结尾（如 UserEntity）
- Controller 以 `XxxController` 结尾，REST 路径用复数
- Service、Repository 按业务领域/子系统分包命名
- Redis Key 格式：{env}:{module}:{business_id}，如 prod:user:123
- PostgreSQL 表名全小写+下划线，字段小写+下划线
- API 响应结构统一为：{ code, message, data }
