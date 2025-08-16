---
trigger: always_on
---

# 数据库设计规范

## 命名约定

### 表名规范
- 使用小写字母和下划线：`marriage_leave_request`
- 使用复数形式：`employees`, `leave_requests`
- 避免缩写，使用完整单词
- 表名应清晰表达业务含义

### 字段名规范
- 使用小写字母和下划线：`employee_id`, `start_date`
- 主键统一命名为 `id`
- 外键格式：`{referenced_table}_id`
- 布尔字段使用 `is_` 前缀：`is_active`, `is_approved`
- 时间字段：`created_at`, `updated_at`, `deleted_at`

### 索引命名
- 主键索引：`pk_{table_name}`
- 外键索引：`fk_{table_name}_{referenced_table}`
- 唯一索引：`uk_{table_name}_{column_name}`
- 普通索引：`idx_{table_name}_{column_name}`

## 数据类型规范

### 字符串类型
- `VARCHAR(50)` - 短文本（姓名、编号等）
- `VARCHAR(255)` - 中等文本（标题、描述等）
- `TEXT` - 长文本（备注、详细描述等）

### 数字类型
- `BIGINT` - 主键ID
- `INTEGER` - 普通整数
- `DECIMAL(10,2)` - 金额（精确到分）
- `SMALLINT` - 状态码、枚举值

### 日期时间类型
- `TIMESTAMP` - 带时区的时间戳
- `DATE` - 纯日期
- `TIME` - 纯时间

### 布尔类型
- `BOOLEAN` - 布尔值


## 备份恢复策略

### 备份频率
- 全量备份：每日凌晨
- 增量备份：每4小时
- 事务日志备份：每15分钟

### 恢复测试
- 每月进行恢复测试
- 验证数据完整性
- 记录恢复时间
