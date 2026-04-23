# Next Fullstack Skills 中文使用指南

适用于 Next.js App Router 项目的 Agent Skills 集合，覆盖前端规范、后端接口规范，以及前后端联调契约规范。

## 仓库提供什么

这个仓库包含 3 个可自动匹配的 background skills，用来帮助模型在不同类型的任务里自动套用对应规范：

- `next-backend-standards`
  用于后端接口、`route.ts`、参数校验、统一返回结构、数据库接入、鉴权、分页、Webhook 等后端规则。
- `next-frontend-standards`
  用于页面开发、组件边界、表单、路由参数、列表筛选、加载态/错误态/空态、页面结构等前端规则。
- `next-fullstack-contracts`
  用于前后端共享契约，例如 DTO、接口返回结构、错误码映射、分页协议、共享 schema、缓存失效协同。

安装后，这 3 个 skills 会更像“后台规则集”而不是手动命令。模型会根据你的任务内容，自动判断该使用哪一个。

## 安装方式

安装整个仓库的全部 skills：

```bash
npx skills add clk-zz/next-fullstack-skills
```

只安装某一个 skill：

```bash
npx skills add clk-zz/next-fullstack-skills --skill next-backend-standards
npx skills add clk-zz/next-fullstack-skills --skill next-frontend-standards
npx skills add clk-zz/next-fullstack-skills --skill next-fullstack-contracts
```

安装前查看仓库里有哪些 skills：

```bash
npx skills add clk-zz/next-fullstack-skills --list
```

## 怎么使用

正常情况下，你不需要手动指定 skill。只要安装完成，模型会根据你的请求自动匹配：

- 如果你是在写接口、写 `route.ts`、接数据库、做鉴权、做 Webhook，应该命中 `next-backend-standards`
- 如果你是在写页面、写表单、写列表、写筛选、处理 loading / error / empty 状态，应该命中 `next-frontend-standards`
- 如果你是在同时处理前后端联调，比如统一返回结构、前端消费接口、错误码映射、分页协议，应该命中 `next-fullstack-contracts`

## 适合的提问方式

下面这些请求都比较适合触发这套 skills：

- “帮我写一个用户接口，带参数校验和 Prisma 入库”
- “帮我写一个用户列表页，带搜索、筛选、分页和加载态”
- “把前后端接口返回统一一下，前端按统一错误码处理”
- “帮我把 Next.js 项目的前后端结构规范化”

## 每个 Skill 分别负责什么

### `next-backend-standards`

入口文件：
[skills/next-backend-standards/SKILL.md](./skills/next-backend-standards/SKILL.md)

主要覆盖：

- Route Handler 结构
- 接口统一返回格式
- Body / Params / Query 校验
- 后端错误处理
- 数据库连接规则
- Service / Repository 分层
- 鉴权与权限控制
- 分页规范
- Webhook 处理规范

### `next-frontend-standards`

入口文件：
[skills/next-frontend-standards/SKILL.md](./skills/next-frontend-standards/SKILL.md)

主要覆盖：

- Server / Client Component 边界
- 页面级数据获取
- 表单与提交流程
- Loading / Empty / Error / Success 状态
- URL 驱动的筛选与分页
- 登录态与权限态页面行为
- 前端页面和组件目录结构

### `next-fullstack-contracts`

入口文件：
[skills/next-fullstack-contracts/SKILL.md](./skills/next-fullstack-contracts/SKILL.md)

主要覆盖：

- DTO 设计
- 前后端共享请求/响应契约
- 错误码与前端提示映射
- 分页请求和响应规范
- 共享 schema 使用边界
- API Client 消费约定
- 缓存失效与刷新协同

## 推荐使用场景

如果你希望在 Next.js 项目里解决下面这些问题，这个仓库会比较合适：

- 前后端都写在一个项目里，但缺少统一规范
- 不同接口返回结构不一致
- 前端页面对接口的消费方式不统一
- 表单、分页、错误处理、鉴权逻辑比较散
- 想让 AI 在写 Next.js 代码时，自动按一套稳定规则执行
