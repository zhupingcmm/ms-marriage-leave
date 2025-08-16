---
trigger: always_on
---

# API 设计规范

## RESTful API 设计原则

### URL 设计规范
- 使用复数名词：`/api/v1/marriage-leaves`
- 使用小写字母和连字符：`/api/v1/leave-requests`
- 版本控制：所有API以 `/api/v1/` 开头
- 资源嵌套不超过2层：`/api/v1/employees/{id}/leave-requests`

### HTTP 方法使用
- `GET` - 查询资源（幂等）
- `POST` - 创建资源
- `PUT` - 完整更新资源（幂等）
- `PATCH` - 部分更新资源
- `DELETE` - 删除资源（幂等）

### 状态码规范
- `200` - 请求成功
- `201` - 创建成功
- `204` - 删除成功（无内容返回）
- `400` - 请求参数错误
- `401` - 未认证
- `403` - 无权限
- `404` - 资源不存在
- `409` - 资源冲突
- `422` - 业务逻辑错误
- `500` - 服务器内部错误

## 统一响应格式

### 成功响应
```json
{
  "code": 200,
  "message": "操作成功",
  "data": {
    // 实际数据
  },
  "timestamp": "2025-08-16T17:05:01+08:00"
}
```

### 错误响应
```json
{
  "code": 400,
  "message": "请求参数错误",
  "data": null,
  "errors": [
    {
      "field": "startDate",
      "message": "开始日期不能为空"
    }
  ],
  "timestamp": "2025-08-16T17:05:01+08:00"
}
```

### 分页响应
```json
{
  "code": 200,
  "message": "查询成功",
  "data": {
    "content": [],
    "page": 0,
    "size": 10,
    "totalElements": 100,
    "totalPages": 10,
    "first": true,
    "last": false
  },
  "timestamp": "2025-08-16T17:05:01+08:00"
}
```

## 请求参数规范

### 查询参数
- 分页：`page=0&size=10`
- 排序：`sort=createdAt,desc`
- 过滤：`status=APPROVED&startDate=2025-01-01`
- 搜索：`search=关键词`

### 请求体格式
- 使用 JSON 格式
- 字段名使用 camelCase
- 日期格式：`yyyy-MM-dd` 或 `yyyy-MM-ddTHH:mm:ss`
- 布尔值：`true/false`

## 婚假管理 API 端点设计

### 婚假申请管理
```
GET    /api/v1/marriage-leaves           # 查询婚假申请列表
POST   /api/v1/marriage-leaves           # 创建婚假申请
GET    /api/v1/marriage-leaves/{id}      # 查询特定婚假申请
PUT    /api/v1/marriage-leaves/{id}      # 更新婚假申请
DELETE /api/v1/marriage-leaves/{id}      # 删除婚假申请
```

### 审批流程
```
POST   /api/v1/marriage-leaves/{id}/approve    # 审批通过
POST   /api/v1/marriage-leaves/{id}/reject     # 审批拒绝
GET    /api/v1/marriage-leaves/{id}/history    # 查询审批历史
```

### 员工管理
```
GET    /api/v1/employees                       # 查询员工列表
GET    /api/v1/employees/{id}/marriage-leaves  # 查询员工的婚假记录
```

## 错误处理规范

### 参数验证错误
- 使用 `@Valid` 注解进行参数验证
- 返回具体的字段错误信息
- 错误码使用 400

### 业务逻辑错误
- 自定义业务异常
- 返回有意义的错误消息
- 错误码使用 422

### 系统错误
- 记录详细日志
- 返回通用错误消息
- 错误码使用 500

## 安全规范

### 认证授权
- 使用 JWT Token 进行身份认证
- 在 Header 中传递：`Authorization: Bearer {token}`
- Token 过期时间：2小时

### 权限控制
- 员工只能查看和操作自己的婚假申请
- 管理员可以查看和审批所有申请
- HR 可以查看统计数据

## 文档要求

### Controller 注释
- 使用 `@Api` 描述控制器用途
- 使用 `@ApiOperation` 描述接口功能
- 使用 `@ApiParam` 描述参数
- 使用 `@ApiResponse` 描述响应

### 示例
```java
@RestController
@RequestMapping("/api/v1/marriage-leaves")
@Api(tags = "婚假管理")
public class MarriageLeaveController {
    
    @GetMapping
    @ApiOperation(value = "查询婚假申请列表", notes = "支持分页和条件查询")
    @ApiResponses({
        @ApiResponse(code = 200, message = "查询成功"),
        @ApiResponse(code = 400, message = "参数错误")
    })
    public ResponseEntity<ApiResponse<Page<MarriageLeaveDto>>> getMarriageLeaves(
        @ApiParam(value = "页码", defaultValue = "0") @RequestParam(defaultValue = "0") int page,
        @ApiParam(value = "页大小", defaultValue = "10") @RequestParam(defaultValue = "10") int size
    ) {
        // 实现逻辑
    }
}
```
