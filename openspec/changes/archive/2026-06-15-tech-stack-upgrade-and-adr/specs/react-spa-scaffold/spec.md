## MODIFIED Requirements

### Requirement: Vite + React + TypeScript 工程结构

系统 SHALL 提供基于 Next.js 15 (App Router) 的 React 19 + TypeScript 前端工程，包含 `app/`（页面与布局）、`components/`（共享组件）、`lib/`（工具库与 API 层）目录结构。

#### Scenario: 工程启动

- **WHEN** 开发者在 `frontend/` 目录执行 `npm install && npm run dev`
- **THEN** 开发服务器 SHALL 在 `http://localhost:3000` 启动，页面正常渲染

#### Scenario: 生产构建

- **WHEN** 开发者执行 `npm run build`
- **THEN** SHALL 在 `.next/` 目录生成可部署的静态与服务端资源

---

### Requirement: React Router 路由配置

系统 SHALL 采用 Next.js App Router 文件路由，页面组件放置于 `app/` 目录下，路由路径与目录结构自动映射，支持 `not-found.tsx` 自定义 404 页面。

#### Scenario: 路由导航

- **WHEN** 用户访问 `/` 路径
- **THEN** SHALL 渲染 `app/page.tsx` 首页组件

#### Scenario: 404 路由

- **WHEN** 用户访问未定义的路径 `/unknown`
- **THEN** SHALL 渲染 `app/not-found.tsx` 404 页面组件

#### Scenario: 客户端导航

- **WHEN** 用户通过布局组件的 Menu 菜单点击导航
- **THEN** SHALL 使用 `useRouter().push()` 进行客户端路由跳转，无需整页刷新

---

### Requirement: Axios HTTP 客户端封装

系统 SHALL 封装 Axios 实例，统一配置 `baseURL`（默认 `/api`）、响应拦截器（统一错误处理、提取 `data` 字段），提供类型安全的 API 调用方法。

#### Scenario: 开发环境代理

- **WHEN** 前端发起 `/api/users` 请求
- **THEN** Next.js rewrites SHALL 将请求代理转发到 `http://localhost:8080/api/users`

#### Scenario: 统一错误处理

- **WHEN** 后端返回 `code` 非 `200` 的 `Result`
- **THEN** 响应拦截器 SHALL 弹出错误提示，并返回 rejected Promise

#### Scenario: 自动解包

- **WHEN** 后端返回 `{ code: 200, message: "success", data: [...] }`
- **THEN** 响应拦截器 SHALL 自动提取 `data` 字段作为 Promise resolve 值

---

### Requirement: Ant Design UI 集成

系统 SHALL 集成 Ant Design 6 组件库，提供全局中文国际化配置，使用 `@ant-design/nextjs-registry` 解决 SSR 样式闪烁问题，包含 `App`、`ConfigProvider`、`AntdRegistry` 等顶层 Provider。

#### Scenario: 中文国际化

- **WHEN** 使用 Ant Design 的 Table、Modal、Pagination 等组件
- **THEN** 组件文案 SHALL 以中文显示（如"第1页/共10页"）

#### Scenario: SSR 样式一致性

- **WHEN** 页面通过服务端渲染加载 Ant Design 组件
- **THEN** 组件样式 SHALL 在首屏完整渲染，无样式闪烁

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

## REMOVED Requirements

### Requirement: Vite 代理配置

**Reason**: Vite 已被 Next.js 替代，代理功能改由 Next.js rewrites 实现
**Migration**: 使用 `next.config.mjs` 中的 `rewrites` 配置替代 `vite.config.ts` 中的 `proxy` 配置