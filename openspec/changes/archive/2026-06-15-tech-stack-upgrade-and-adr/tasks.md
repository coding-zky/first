## 1. 技术选型 ADR 文档化

- [ ] 1.1 在 AGENTS.md 中添加"技术选型约束"章节，包含团队与项目硬约束表（7条约束）
- [ ] 1.2 在 AGENTS.md 中添加前端选型决策表（Next.js vs Nuxt 五维评分对比）
- [ ] 1.3 在 AGENTS.md 中添加后端选型决策表（SpringBoot vs API Routes 六维评分对比）
- [ ] 1.4 在 AGENTS.md 中定义选型红线（3条禁止项 + ADR 变更流程）

## 2. 前端框架迁移（Vite → Next.js）

- [ ] 2.1 删除 Vite 相关文件：index.html、vite.config.ts、src/main.tsx、src/App.tsx、src/router/index.tsx、src/vite-env.d.ts
- [ ] 2.2 更新 package.json：移除 vite/react-router-dom，新增 next@^15.3.9、@ant-design/nextjs-registry
- [ ] 2.3 更新 tsconfig.json：jsx 改为 preserve，添加 next 插件，配置 @/* 路径别名
- [ ] 2.4 创建 next.config.mjs：配置 rewrites 代理 /api 到 http://localhost:8080，添加 outputFileTracingRoot
- [ ] 2.5 创建 app/layout.tsx：根布局，包含 AntdRegistry + ConfigProvider（zhCN）+ AppLayout
- [ ] 2.6 创建 components/AppLayout.tsx：布局组件，使用 useRouter/usePathname 替代 react-router-dom
- [ ] 2.7 创建 app/page.tsx：首页组件（'use client'）
- [ ] 2.8 创建 app/users/page.tsx：用户管理页面（'use client'）
- [ ] 2.9 创建 app/not-found.tsx：404 页面组件
- [ ] 2.10 创建 app/globals.css：全局样式
- [ ] 2.11 创建 lib/store.ts：Zustand store（useStore hook）
- [ ] 2.12 创建 lib/api/request.ts：Axios 封装（baseURL=/api，响应拦截器，自动解包 data）
- [ ] 2.13 创建 lib/api/user.ts：用户 API 方法

## 3. 依赖版本升级至最新稳定版

- [ ] 3.1 升级 React 18 → 19.2.7（react + react-dom）
- [ ] 3.2 升级 Next.js 14 → 15.3.9（实际解析至 15.5.x）
- [ ] 3.3 升级 Ant Design 5 → 6.4.4
- [ ] 3.4 升级 @ant-design/icons 5 → 6.2.5
- [ ] 3.5 升级 @ant-design/nextjs-registry 1.0 → 1.3.0
- [ ] 3.6 升级 zustand 5.0.0 → 5.0.14
- [ ] 3.7 升级 axios 1.7.9 → 1.18.0
- [ ] 3.8 升级 TypeScript 5.6 → 5.8.3、@types/react 18 → 19.1.8

## 4. 验证与构建

- [ ] 4.1 在 frontend/ 目录执行 npm install 安装所有依赖
- [ ] 4.2 执行 npm run build 验证 Next.js 15 生产构建成功（无 error，无 multiple lockfiles 警告）
- [ ] 4.3 确认 app/ 目录结构符合 Next.js App Router 约定
- [ ] 4.4 确认所有使用 hooks 的组件已添加 'use client' 指令

## 5. Git 提交与同步

- [ ] 5.1 在 frontend/ 目录 git add + git commit（迁移 + 升级变更）
- [ ] 5.2 git push frontend submodule 到 first_frontend.git
- [ ] 5.3 在根目录 git add frontend（更新 submodule pointer）
- [ ] 5.4 git add AGENTS.md（技术选型约束章节）
- [ ] 5.5 根目录 git commit + git push 到 first.git
