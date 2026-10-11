# 白底工坊 WhiteCut

> 抠图 → 背景留白 → 自动裁边 →（可选）矢量化，一站式浏览器端工作台。
> 为专业矢量化处理流程设计的素材预处理工具。

**线上地址（GitHub Pages）**：<https://yiwanpro.github.io/whitecut/>

## 这是什么

把「上传图片 → AI 抠出主体 → 强制纯白背景 → 裁边留白 → 导出 PNG/JPG/SVG」这一系列重复操作，
封装成一个网页工具。默认引擎完全在浏览器本地运行，图片不经过任何服务器。

## 功能

- **三引擎抠图**
  - 🧩 **本地 AI 抠图**（默认）：开源模型（ISNet）直接在浏览器运行，免费、无需 Key、可批量、像素级保真；
  - ✨ **Gemini 精修**：填入你自己的 Gemini API Key（AI Studio 申请），使用可编辑的提示词模板处理，适合复杂/有水印场景；
  - 📥 **手动导入**：承接「即梦」工作流 —— 把即梦处理好的白底图拖进来（**含左上角水印也能处理**）。
- **智能水印链**（解决 Gemini × 即梦 的繁琐流程）
  - Gemini 抠图失败时，自动先执行「去水印」提示词，再重新抠图；
  - 手动导入的即梦白底图，若配置了 Key，自动先去除左上角水印；
  - 每张图都可在详情里对结果单独执行「用 Gemini 去除水印」。
- **白化处理器**（本地像素级，不重绘）
  - 白底图自动反推透明通道（边界泛洪算法，不误伤主体内部白色区域）；
  - 边缘收缩 / 平滑（去毛边、去光晕）；
  - 强制纯白阈值（把近白像素提到 #FFFFFF，满足印刷/印刷级素材要求）；
  - 自动裁边 + 可调留白边距，支持正方形画布输出。
- **矢量化**：单色/线稿、彩色两种模式，输出 SVG；
- **批量**：多图排队处理，一键打包 ZIP（白底 PNG + 透明 PNG + SVG）。

## 使用

1. 打开线上地址（或本地运行）；
2. 拖拽 / 粘贴（Ctrl+V）/ 点击上传图片，可多选；
3. 选引擎（默认本地 AI）→ 点「开始处理」；
4. 点击卡片查看对比详情，微调参数后可「重新白化」，满意后下载；
5. 批量结果点底部「打包下载 ZIP」。

首次使用本地引擎需下载约 40MB 模型文件（浏览器会缓存，之后离线可用）。

### 配置 Gemini（可选）

1. 用你的 Google 账号（如 yiwanpro@gmail.com）登录 [AI Studio](https://aistudio.google.com) → Get API key；
2. 复制 Key → 打开工具右上角「设置」→ 粘贴保存；
3. API Key 与网站域名无关，在任何站点都可以使用；Key 只保存在你本机浏览器里。

> 提示：直连 Google 需要网络可达。无法直连时请使用「本地 AI」或「手动导入」引擎。

## 部署

### 方式一：GitHub Pages（已开启）

仓库推送即部署：<https://yiwanpro.github.io/whitecut/>

### 方式二：嵌入自己的网站（huangyiwan.com，Astro 静态站）

把 `index.html` 单文件放进 Astro 项目的 `public/` 目录：

```bash
# 在你的博客仓库里
mkdir -p public/whitecut
cp index.html public/whitecut/index.html
```

访问 `https://huangyiwan.com/whitecut/` 即可，同域名直开、无需 iframe。

或在任意页面嵌入 iframe（比如写在某篇博客文章里）：

```html
<iframe src="/whitecut/" style="width:100%;height:860px;border:0;border-radius:12px" loading="lazy" title="白底工坊"></iframe>
```

### 本地运行

```bash
python3 -m http.server 8080
# 打开 http://localhost:8080
```

> 必须通过 http 访问（浏览器对 file:// 的模块加载有安全限制）。

## 隐私

- 「本地 AI」与「手动导入」引擎：所有图片与处理全部在你的浏览器内完成，零上传；
- 「Gemini 精修」：图片会发送到 Google 的 API（使用你自己的 Key）；API Key 仅存 localStorage。

## 许可

MIT License。本地模型基于 ISNet（@imgly/background-removal）。
