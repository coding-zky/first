## ADDED Requirements

### Requirement: Next.js App Router 工程结构

系统 SHALL 提供基于 Next.js 15 (App Router) 的 React 19 + TypeScript 前端工程，包含 `app/`（页面与布局）、`components/`（共享组件）、`lib/`（工具库与 API 层）目录结构。

#### Scenario: 工程启动

- **WHEN** 开发者在 `frontend/` 目录执行 `npm install && npm run dev`
- **THEN** 开发服务器 SHALL 在 `http://localhost:3000` 启动，页面正常渲染

#### Scenario: 生产构建

- **WHEN** 开发者执行 `npm run build`
- **THEN** SHALL 在 `.next/` 目录生成可部署的静态与服务端资源

---

### Requirement: Next.js 文件路由

系统 SHALL 采用 Next.js App Router 文件路由，页面组件放置于 `app/` 目录下，路由路径与目录结构自动映射，无需手动配置路由表。

#### Scenario: 首页路由

- **WHEN** 用户访问 `/` 路径
- **THEN** SHALL 渲染 `app/page.tsx` 首页组件

#### Scenario: 子页面路由

- **WHEN** 用户访问 `/users` 路径
- **THEN** SHALL 渲染 `app/users/page.tsx` 用户管理组件

#### Scenario: 404 页面

- **WHEN** 用户访问未定义的路径 `/unknown`
- **THEN** SHALL 渲染 `app/not-found.tsx` 404 页面组件

---

### Requirement: AntdRegistry SSR 样式支持

系统 SHALL 使用 `@ant-design/nextjs-registry` 包裹 Ant Design 组件，解决 Next.js SSR 场景下 Ant Design 样式闪烁（FOUC）问题。

#### Scenario: SSR 样式一致性

- **WHEN** 页面通过服务端渲染加载 Ant Design 组件
- **THEN** 组件样式 SHALL 在首屏完整渲染，无样式闪烁

---

### Requirement: Next.js Rewrites API 代理

系统 SHALL 在 `next.config.mjs` 中配置 rewrites，将 `/api` 前缀的请求代理转发到 SpringBoot 后端（`http://localhost:8080`），解决开发环境跨域问题。

#### Scenario: API 请求代理

- **WHEN** 前端代码调用 `axios.get('/api/users')`
- **THEN** Next.js rewrites SHALL 将请求代理转发到 `http://localhost:8080/api/users`

---

### Requirement: Client Components 标记

系统 SHALL 在所有使用 React hooks、浏览器 API 或事件处理的组件文件顶部添加 `'use client'` 指令，确保 Next.js 正确识别客户端组件边界。

#### Scenario: 页面组件使用 hooks

- **WHEN** 页面组件（如 UserListPage）使用 useState、useEffect 等 hooks
- **THEN** 该组件文件 SHALL 在顶部包含 `'use client'` 指令

#### Scenario: 布局组件使用路由 hooks

- **WHEN** 布局组件使用 useRouter、usePathname 等 Next.js 路由 hooks
- **THEN** 该组件 SHALL 标记为 `'use client'`