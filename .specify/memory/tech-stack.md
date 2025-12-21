# Math Mate 技术栈

**前端**: Vue 3 + TypeScript + PrimeVue + Tailwind CSS + Pinia + Vite
**后端**: Go + Gin + GORM + SQLite + Eino Agent

## 前端技术栈

- **Vue 3 Composition API** + **TypeScript**
- **PrimeVue** UI组件库（支持主题定制，适配移动端）
- **Tailwind CSS** 样式框架（与PrimeVue配合）
- **Pinia** 状态管理
- **KaTeX** 数学公式渲染（轻量级，高性能，适合高中数学公式和实时交互）
- **Vite** 构建工具

## 后端技术栈

### 核心框架
- **Go** + **Gin** Web框架
- **Eino Agent** 框架（智能代理应用开发）
- **SQLite** + **GORM**（轻量级数据库方案）

### API文档
- **Swagger/OpenAPI** 自动生成API文档

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

## 第三方集成

- **AI/LLM服务**: 通过Eino框架集成，支持对话生成和多轮上下文
- **OCR服务**: 图像识别（拍照题目识别）
  - 详细技术方案请参考 [OCR和语音识别技术方案](ocr_voice_solution.md)
- **语音识别**: 语音转文本（口语化数学表达）
  - 详细技术方案请参考 [OCR和语音识别技术方案](ocr_voice_solution.md)

## 前端架构说明

**services/**: 业务逻辑服务层（核心）
- 处理复杂业务逻辑，协调stores/api/utils调用
- 组件通过services访问业务逻辑，不直接操作stores
- 确保业务逻辑与UI分离，便于测试和复用

**stores/**: Pinia状态管理，仅存储全局状态和简单操作

## 版本要求

**Go**: 1.25+
**前端核心**: Vue 3 + TypeScript + PrimeVue + Tailwind CSS

**相关文档**: 编码规范见 `coding-standards.md`

