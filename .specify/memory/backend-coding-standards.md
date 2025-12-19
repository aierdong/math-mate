# 后端编码规范

**版本**: 1.0.0  
**最后更新**: 2025-12-19  
**适用范围**: Go + Eino Agent + GORM + SQLite

## 核心约束

### 项目结构规范

遵循以下目录结构：

```
backend/
├── cmd/                # 应用入口
├── internal/           # 内部代码（不对外暴露）
│   ├── handlers/       # HTTP 处理器
│   ├── services/       # 业务逻辑
│   ├── models/         # 数据模型
│   ├── repositories/   # 数据访问层
│   ├── agents/         # Eino Agent 相关代码
│   ├── middleware/     # HTTP 中间件
│   └── config/         # 配置管理
├── pkg/                # 可复用的公共包
├── migrations/         # 数据库迁移
└── tests/              # 测试文件
```

### 错误处理规范

- **必须**检查所有错误，使用 `fmt.Errorf` 和 `%w` 包装错误
- 定义错误变量用于错误比较（如 `ErrUserNotFound`）
- HTTP 错误响应使用结构化格式（`HTTPError`）

### 数据库规范

- 模型放在 `internal/models/`，使用 GORM 标签
- 使用 Repository 模式：接口定义在 service 包，实现放在 `internal/repositories/`
- 使用 `Select` 指定字段，`Preload` 预加载关联，避免 N+1 查询

### API 设计规范

- RESTful API：URL 使用名词复数形式，标准 HTTP 方法
- 请求/响应结构体定义在 handler 包
- 路由组织：使用路由组，中间件处理认证、日志

### Eino Agent 规范

- Agent 代码放在 `internal/agents/` 目录
- Agent 配置从环境变量读取（敏感信息如 API Key）
- 配置结构体放在 `internal/config/` 目录

### 测试规范

- 测试文件使用 `*_test.go` 后缀
- 使用 `testify` 增强断言
- 测试覆盖率目标：**80%** 以上

### 日志规范

- 使用结构化日志（JSON 格式）
- 包含上下文信息（请求 ID、用户 ID 等）
- 敏感信息不记录到日志

## 相关文档

- **技术栈文档**: `.specify/memory/tech-stack.md`
- **前端编码规范**: `.specify/memory/frontend-coding-standards.md`