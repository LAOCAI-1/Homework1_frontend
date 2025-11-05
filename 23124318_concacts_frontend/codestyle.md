# Code Style Guide — Frontend (Concacts)

> **参考来源（必读）**
>
> - Airbnb JavaScript Style Guide: https://github.com/airbnb/javascript
> - MDN Web Docs (HTML/CSS/JS): https://developer.mozilla.org/
> - BEM 命名规范（Yandex）: https://en.bem.info/methodology/
> - Prettier 格式化规范: https://prettier.io/docs/en/options.html

---

## 1. 基本约定

- **语言/语法**：ES6+ 原生 JavaScript；不使用框架（本作业限定）。
- **编码**：UTF-8；文件末尾保留换行。
- **缩进**：2 空格；**禁止** Tab。
- **行宽**：建议 ≤ 100 列。
- **换行**：LF（`\n`），Windows 下 Git 统一转 LF。
- **分号**：统一 **使用分号**。
- **引号**：JavaScript 使用 **单引号**，HTML 属性无特别要求；字符串内含引号时可灵活处理。
- **命名**：
  - 变量/函数：`camelCase`
  - 常量：`UPPER_SNAKE_CASE`
  - 文件：`kebab-case`（如 `app.js`, `me.js`）
  - CSS 类名：**BEM**（`block__element--modifier`）

---

## 2. HTML 规范

- **结构语义化**：优先使用语义标签（`header/main/section/nav/footer`）。
- **可访问性（A11y）**：
  - 图片加 `alt`；交互元素必须可聚焦（`button`/`a`/`tabindex`）。
  - 表单控件有 `label`；弹层有 `aria-*` 合理标注。
- **属性顺序**：`id` → `class` → `name` → `data-*` → 其他。
- **资源引入**：脚本统一放文档底部，或使用 `defer`。
- **避免内联样式/脚本**：页面中最多只保留少量启动脚本，其余独立文件：`app.js`、`me.js`、`config.js`。

---

## 3. CSS 规范

- **层叠设计**：组件化 + BEM；避免全局样式污染。
- **变量**：统一使用 `:root { --var-name: value; }` 管理主题色与间距。
- **单位**：字体 `rem`，间距 `px`/`rem`；禁止 `!important`（除非临时热修）。
- **顺序**：定位/布局 → 尺寸 → 排版 → 视觉（颜色、背景、阴影）。
- **响应式**：移动优先；合理使用媒体查询。
- **禁用点**：不使用过深层级选择器（建议 ≤ 3 层）。

**命名示例：**

```css
.card {
}
.card__header {
}
.card__body {
}
.card--danger {
}
```

## 4. JavaScript 规范

**模块结构：**

- config.js：仅暴露 API_BASE 等常量；

- app.js：通讯录主逻辑（列表、搜索、弹层、新增/编辑/删除、合并提示）；

- me.js：个人资料面板逻辑。

- 变量与常量：优先 const，需要重赋值再用 let；禁止使用 var。

- 解构/模板串：大量字符串拼接使用模板字符串 `${}`。

- 事件绑定：addEventListener，避免 HTML onclick。

- DOM 查询：封装工具 const $ = (s)=>document.querySelector(s)。

- 异步：统一 async/await，所有 fetch 必做 .ok 判断与异常兜底。

- 错误处理：用户可感知的错误 alert 或轻提示；控制台输出用于开发排查。

- 本地存储：localStorage 仅存 Token 与少量用户非敏感信息，Key 统一命名（如 token/user）。

- 安全：所有请求需带 Authorization: Bearer <token>；避免在前端写死敏感密钥。

- 示例：防呆式 Fetch

```
async function apiGet(url, headers = {}) {
const res = await fetch(url, { headers });
const data = await res.json().catch(() => ({}));
if (!res.ok) throw new Error(data.error || 'Request failed');
return data;
}
```

## 5. 交互一致性

- 所有弹层、按钮、输入框风格保持一致（圆角、阴影、间距）。

- 按首字母分组，右侧索引条跳转；空状态需明确文案。

- 同名合并：先提示，再“合并/取消”，合并 UI 与主页面风格一致。

- 退出：清理本地缓存 Token，再跳转登录页。
