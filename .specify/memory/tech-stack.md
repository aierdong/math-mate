# Math Mate 技术栈文档

## 概述

本文档定义了 Math Mate 项目的技术栈选择，包括前端、后端、开发工具、测试框架等所有技术组件。所有功能规划和任务分解都应参考本文档，确保技术选择的一致性。

**版本**: 1.1.0  
**最后更新**: 2025-12-19

## 前端技术栈

### 核心框架
- **Vue 3**: 采用 Vue 3 Composition API，提供响应式数据和组件化开发
- **TypeScript**: 使用 TypeScript 进行类型安全的前端开发，提升代码质量和可维护性

### UI 组件库
- **PrimeVue**: 使用 PrimeVue 作为主要 UI 组件库，提供丰富的企业级组件
  - 支持主题定制，符合项目"活泼明亮"的界面要求
  - 提供响应式布局组件，适配移动端浏览

### 状态管理
- **Pinia**: 使用 Pinia 进行全局状态管理
  - 替代 Vuex，提供更简洁的 API
  - 支持 TypeScript，类型安全

### 构建工具
- **Vite**: 使用 Vite 作为构建工具和开发服务器
  - 快速的 HMR（热模块替换）
  - 优化的生产构建

### 样式方案
- **Tailwind CSS**: 使用 Tailwind CSS 作为主要 CSS 框架
  - 实用优先的 CSS 框架，提供灵活的样式工具类
  - 支持响应式设计和主题定制
  - 与 PrimeVue 组件库配合使用
- **PrimeVue 主题系统**: 利用 PrimeVue 的主题定制能力

### 路由
- **Vue Router**: 使用 Vue Router 进行前端路由管理

### HTTP 客户端
- **Axios**: 使用 Axios 进行 HTTP 请求
  - 统一的请求/响应拦截器
  - 错误处理和重试机制

### 数学公式渲染
- **KaTeX** 或 **MathJax**: 用于渲染 LaTeX 数学公式
  - 支持专业级公式渲染（符合 F-I4 需求）

### 前端测试
- **Vitest**: 使用 Vitest 进行单元测试（与 Vite 集成良好）
- **Vue Test Utils**: Vue 组件测试工具
- **Playwright** 或 **Cypress**: 端到端测试框架（可选）

## 后端技术栈

### 编程语言
- **Golang (Go)**: 使用 Go 语言进行后端开发
  - 高并发性能
  - 简洁的语法和强大的标准库

### Agent 框架
- **Eino**: 使用 Eino Agent 框架构建智能代理应用
  - 支持大模型应用开发
  - 提供 Agent 编排和管理能力
  - 支持多轮对话和上下文管理

### 数据库
- **SQLite**: 使用 SQLite 作为嵌入式数据库
  - 轻量级，适合中小型应用
  - 简化部署和维护
  - 支持事务和 ACID 特性

### ORM/数据库工具
- **GORM**: 使用 GORM 作为 Go 的 ORM 框架
  - 简化数据库操作
  - 支持迁移和模型定义

### Web 框架
- **Gin** 或 **Echo**: 使用轻量级 Web 框架（根据 Eino 框架的集成需求选择）
  - RESTful API 开发
  - 中间件支持

### API 文档
- **Swagger/OpenAPI**: 使用 Swagger 生成 API 文档
  - 符合 API 优先设计原则
  - 自动生成 API 文档

### 后端测试
- **Go testing**: 使用 Go 标准测试框架
- **testify**: 使用 testify 增强测试断言
- **httptest**: 使用 httptest 进行 HTTP 接口测试

## 开发工具与基础设施

### 版本控制
- **Git**: 使用 Git 进行版本控制
- **GitHub/GitLab**: 代码托管平台

### 包管理
- **前端**: npm 或 pnpm（推荐 pnpm，更快的安装速度）
- **后端**: Go Modules（Go 1.11+ 内置）

### 代码质量
- **前端**:
  - **ESLint**: JavaScript/TypeScript 代码检查
  - **Prettier**: 代码格式化
  - **TypeScript**: 静态类型检查
- **后端**:
  - **golangci-lint**: Go 代码静态分析
  - **go fmt**: Go 代码格式化

### 构建与部署
- **Docker**: 使用 Docker 容器化应用（可选）
- **CI/CD**: GitHub Actions 或 GitLab CI/CD
  - 自动化测试
  - 自动化构建和部署

### 环境管理
- **前端**: `.env` 文件管理环境变量
- **后端**: 环境变量或配置文件（支持多环境：dev/staging/prod）

## 第三方服务集成

### AI/LLM 服务
- **大模型 API**: 根据 Eino 框架的要求集成大模型服务
  - 支持对话生成
  - 支持多轮对话上下文

### OCR 服务（可选）
- **图像识别**: 用于拍照识别题目（F-I3 需求）
  - 可考虑集成第三方 OCR API（如百度 OCR、腾讯 OCR 等）

### 语音识别服务（可选）
- **语音转文本**: 用于语音交互（F-I2 需求）
  - 可考虑集成第三方语音识别 API

## 项目结构约定

### 前端项目结构
```
frontend/
├── src/
│   ├── components/      # Vue 组件
│   ├── views/          # 页面视图
│   ├── stores/         # Pinia stores (状态管理)
│   ├── services/       # 业务逻辑服务层
│   │                   # - 处理复杂的业务逻辑
│   │                   # - 调用 stores 进行状态管理
│   │                   # - 调用 utils 使用工具函数
│   │                   # - 协调多个 stores 和 API 调用
│   ├── router/         # 路由配置
│   ├── api/            # API 调用 (HTTP 请求封装)
│   ├── utils/          # 工具函数 (纯函数、辅助方法)
│   ├── types/          # TypeScript 类型定义
│   └── assets/         # 静态资源
├── public/             # 公共资源
├── tests/              # 测试文件
└── package.json
```

**前端架构说明**:
- **services/**: 业务逻辑服务层，处理复杂的业务逻辑
  - 服务层可以调用 `stores/` 进行状态管理
  - 服务层可以调用 `utils/` 使用工具函数
  - 服务层可以调用 `api/` 进行 HTTP 请求
  - 组件和视图层应通过 services 访问业务逻辑，而非直接操作 stores
- **stores/**: Pinia 状态管理，存储全局状态和简单的状态操作
- **utils/**: 纯函数和工具方法，不包含业务逻辑
- **api/**: HTTP 请求封装，负责与后端 API 通信

### 后端项目结构
```
backend/
├── cmd/                # 应用入口
├── internal/           # 内部代码
│   ├── handlers/       # HTTP 处理器
│   ├── services/       # 业务逻辑
│   ├── models/         # 数据模型
│   ├── repositories/   # 数据访问层
│   └── agents/         # Eino Agent 相关代码
├── pkg/                # 可复用的公共包
├── migrations/         # 数据库迁移
├── tests/              # 测试文件
└── go.mod
```

## 技术决策原则

1. **简单性原则**: 优先选择简单、成熟的技术方案
2. **类型安全**: 前后端都使用强类型语言（TypeScript + Go）
3. **API 优先**: 前后端通过 RESTful API 通信，符合 API 优先设计原则
4. **可测试性**: 所有技术选择都应支持测试优先开发
5. **可维护性**: 选择有良好文档和社区支持的技术

## 依赖版本管理

### 前端依赖
- Vue 3: ^3.x
- TypeScript: ^5.x
- PrimeVue: ^3.x 或 ^4.x（根据项目需求）
- Pinia: ^2.x
- Vite: ^5.x
- Tailwind CSS: ^3.x

### 后端依赖
- Go: 1.21+（根据 Eino 框架要求）
- GORM: ^1.x
- SQLite: 通过 GORM 驱动

## 更新历史

- **1.1.0** (2025-12-19): 
  - 更新样式方案为 Tailwind CSS
  - 添加前端 services 目录说明，明确业务逻辑服务层职责
- **1.0.0** (2025-12-19): 初始版本，定义基础技术栈

## 相关文档

- **项目章程**: `.specify/memory/constitution.md`
- **项目描述**: `.specify/memory/project-description.md`
- **领域模型**: `.cursor/rules/01-domain-model.mdc`

