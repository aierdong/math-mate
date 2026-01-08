# Math Mate 编码规范

## 前端规范 (Vue 3 + TypeScript + PrimeVue + Tailwind)

### Vue组件规范
- **必须**使用 Composition API（`<script setup>`）
- Props/Emits 使用 TypeScript 接口定义，`withDefaults` 设置默认值
- 组件内部优先使用 `ref`，复杂对象除外

### 文件组织
- 使用 4 个空格缩进，每行不超过 120 字符
- 导入顺序：Vue相关 → 第三方库 → 类型定义 → 项目模块
- 使用路径别名 `@/` 导入项目文件

### 架构模式
- **services/**: 业务逻辑服务层，处理复杂逻辑，调用 stores/api/utils
- **stores/**: Pinia 状态管理，仅存储全局状态和简单操作
- **组件通过 services 访问业务逻辑，不直接操作 stores**

### 样式规范
- **优先**使用 Tailwind CSS 工具类
- 组件样式使用 `scoped`
- 避免直接覆盖 PrimeVue 内部样式

## 后端规范 (Go + GORM + SQLite)

### 项目结构
```
backend/
├── cmd/                # 应用入口
├── internal/           # 内部代码
│   ├── handlers/       # HTTP 处理器
│   ├── services/       # 业务逻辑
│   ├── models/         # 数据模型
│   ├── repositories/   # 数据访问层
│   └── agents/         # Eino Agent 相关代码
├── pkg/                # 可复用公共包
└── migrations/         # 数据库迁移
```

### 错误处理
- **必须**检查所有错误，使用 `fmt.Errorf` 和 `%w` 包装
- 定义错误变量用于错误比较（如 `ErrUserNotFound`）
- HTTP 错误响应使用结构化格式

### 数据库规范
- 模型放在 `internal/models/`，使用 GORM 标签
- 使用 Repository 模式：接口定义在 service 包，实现放在 repositories
- 使用 `Select` 指定字段，`Preload` 预加载关联，避免 N+1 查询

### API设计
- RESTful API：URL 使用名词复数，标准 HTTP 方法
- 请求/响应结构体定义在 handler 包

### 测试规范
- 测试文件使用 `*_test.go` 后缀
- 使用 `testify` 增强断言
- 测试覆盖率目标：80% 以上

**相关文档**: 技术栈详见 `tech-stack.md`，项目章程见 `constitution.md`
