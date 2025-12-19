# 前端编码规范

**版本**: 1.0.0  
**最后更新**: 2025-12-19  
**适用范围**: Vue 3 + TypeScript + PrimeVue + Tailwind CSS

## 核心约束

### Vue 3 组件规范

- **必须**使用 Composition API（`<script setup>`），禁止使用 Options API
- 组件结构顺序：imports → props → emits → composables → 响应式数据 → 计算属性 → 方法 → 生命周期
- Props 和 Emits 使用 TypeScript 接口定义，使用 `withDefaults` 设置默认值
- 优先使用 `ref` 而非 `reactive`（复杂对象除外）

### 文件组织规范

- 使用 **4 个空格**进行缩进
- 每行代码长度不超过 **120 个字符**
遵循以下目录结构：

```
frontend/src/
├── components/      # 可复用组件（PascalCase 文件名）
├── views/           # 页面视图
├── stores/          # Pinia stores（camelCase，以 Store 结尾）
├── services/        # 业务逻辑服务层（重要：组件通过 services 访问业务逻辑）
├── router/          # 路由配置
├── api/             # API 调用封装
├── utils/           # 工具函数（纯函数）
├── types/           # TypeScript 类型定义
└── assets/          # 静态资源
```

**关键架构原则**：
- **services/**: 处理复杂业务逻辑，可调用 stores、api、utils
- **stores/**: 存储全局状态和简单状态操作
- **组件和视图**: 通过 services 访问业务逻辑，不直接操作 stores

### 导入路径

- 使用路径别名 `@/` 导入项目内文件
- 导入顺序：Vue 相关 → 第三方库 → 类型定义（`import type`） → 项目内模块

### 状态管理规范

- Store 使用 `storeToRefs` 解构响应式状态
- 组件中不直接修改 store state，通过 actions 修改
- Store 应该是单一职责

### API 调用规范

- 所有 API 调用封装在 `src/api/` 目录
- 使用 Axios 实例，配置统一拦截器
- API 函数返回 Promise，使用 TypeScript 类型

### 样式规范

- **优先**使用 Tailwind CSS 工具类
- 避免自定义 CSS，除非 Tailwind 无法实现
- 组件样式使用 `scoped`
- 使用 PrimeVue 内置样式类，避免直接覆盖内部样式

### 错误处理

- 使用自定义错误类（`ApiError`、`ValidationError`）
- 使用 PrimeVue Toast 显示错误消息

## 相关文档

- **技术栈文档**: `.specify/memory/tech-stack.md`
- **后端编码规范**: `.specify/memory/backend-coding-standards.md