# 代码风格偏好

## Python

- [ ] **FastAPI + SQLite** 做轻量后端，不上 PostgreSQL/MySQL 除非必要。
- [ ] **Pydantic 做数据校验。**
- [ ] **CORS 全开。** 个人项目 allow_origins=["*"]。
- [ ] **API 路径统一 `/api/` 前缀。**
- [ ] **提供一个 `/api/summary` 聚合接口。** 前端一次请求拿到所有数据，减少请求数。

## JavaScript

- [ ] **不用 TypeScript。** 个人项目直接写 JS。
- [ ] **不用构建工具。** 不要 webpack/vite/rollup，直接浏览器跑。
- [ ] **Alpine.js 做交互。** x-data、x-show、x-for 够用了。
- [ ] **变量命名用 camelCase。**

## 通用

- [ ] **注释写中文。** 代码注释和文档用中文。
- [ ] **文件编码 UTF-8。**
- [ ] **Git commit message 用英文。** 格式：`type: description`（如 `feat: add PWA support`）。
