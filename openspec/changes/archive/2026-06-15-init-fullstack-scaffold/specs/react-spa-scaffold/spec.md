## ADDED Requirements

### Requirement: Vite + React + TypeScript 工程结构

系统 SHALL 提供基于 Vite 的 React + TypeScript 前端工程，包含 `src/components`（通用组件）、`src/pages`（页面组件）、`src/api`（API 请求层）、`src/router`（路由配置）、`src/store`（状态管理）、`src/utils`（工具函数）目录结构。

#### Scenario: 工程启动

- **WHEN** 开发者在 `frontend/` 目录执行 `npm install && npm run dev`
- **THEN** 开发服务器 SHALL 在 `http://localhost:5173` 启动，页面正常渲染

#### Scenario: 生产构建

- **WHEN** 开发者执行 `npm run build`
- **THEN** SHALL 在 `dist/` 目录生成可部署的静态资源文件

---

### Requirement: React Router 路由配置

系统 SHALL 集成 React Router v6，提供集中式路由配置，支持页面级懒加载（`React.lazy` + `Suspense`），包含首页和 404 页面路由。

#### Scenario: 路由导航

- **WHEN** 用户访问 `/` 路径
- **THEN** SHALL 渲染首页组件

#### Scenario: 404 路由

- **WHEN** 用户访问未定义的路径 `/unknown`
- **THEN** SHALL 渲染 404 页面组件

#### Scenario: 懒加载

- **WHEN** 用户首次访问某个页面路由
- **THEN** 该页面的 JS 包 SHALL 在该时刻异步加载，不在首屏加载

---

### Requirement: Axios HTTP 客户端封装

系统 SHALL 封装 Axios 实例，统一配置 `baseURL`（默认 `/api`）、请求拦截器（添加公共请求头）、响应拦截器（统一错误处理、提取 `data` 字段），提供类型安全的 API 调用方法。

#### Scenario: 开发环境代理

- **WHEN** 前端发起 `/api/users` 请求
- **THEN** Vite 代理 SHALL 将请求转发到 `http://localhost:8080/api/users`

#### Scenario: 统一错误处理

- **WHEN** 后端返回 `code` 非 `200` 的 `Result`
- **THEN** 响应拦截器 SHALL 弹出错误提示，并返回 rejected Promise

#### Scenario: 自动解包

- **WHEN** 后端返回 `{ code: 200, message: "success", data: [...] }`
- **THEN** 响应拦截器 SHALL 自动提取 `data` 字段作为 Promise resolve 值

---

### Requirement: Ant Design UI 集成

系统 SHALL 集成 Ant Design 5 组件库，提供全局中文国际化配置，包含 `App`、`ConfigProvider` 等顶层 Provider。

#### Scenario: 中文国际化

- **WHEN** 使用 Ant Design 的 Table、Modal、Pagination 等组件
- **THEN** 组件文案 SHALL 以中文显示（如"第1页/共10页"）

#### Scenario: 消息提示

- **WHEN** 调用 Ant Design 的 `message.success("操作成功")`
- **THEN** SHALL 在页面顶部显示绿色的成功提示

---

### Requirement: Zustand 状态管理

系统 SHALL 集成 Zustand 进行轻量级状态管理，用于存储全局用户信息、认证状态等，提供 `useStore` hook 供组件使用。

#### Scenario: 全局状态读写

- **WHEN** 组件调用 `useStore(state => state.user)` 读取用户信息
- **THEN** SHALL 获取到全局存储的用户对象

#### Scenario: 状态更新

- **WHEN** 登录成功后调用 `useStore.getState().setUser(userInfo)`
- **THEN** 所有使用 `useStore` 的组件 SHALL 自动重渲染获取最新用户信息

---

### Requirement: Vite 代理配置

系统 SHALL 在 Vite 配置中设置开发代理，将 `/api` 前缀的请求转发到 SpringBoot 后端（`http://localhost:8080`），解决开发环境跨域问题。

#### Scenario: API 请求代理

- **WHEN** 前端代码调用 `axios.get('/api/users')`
- **THEN** Vite 开发服务器 SHALL 将请求代理转发到 `http://localhost:8080/api/users`
