# RECOVERY PLUS — Theratools 空气压力波运动恢复设备官网

面向运动员的高压空气压缩恢复靴（空气动力靴）中文营销官网，采用 **React 19 + TypeScript + Tailwind CSS + Vite** 构建。

设计风格还原自原始设计稿，并在此基础上补充了滚动视差、GSAP 动效与响应式布局。

## 技术栈

| 分类 | 选型 |
| --- | --- |
| 框架 | React 19 |
| 语言 | TypeScript |
| 构建工具 | Vite 8 |
| 样式 | Tailwind CSS 3 + PostCSS + Autoprefixer |
| 路由 | React Router DOM 7 |
| 动效 | GSAP 3 + `@gsap/react` |
| 代码检查 | oxlint |

## 环境要求

- Node.js 20 及以上（建议 LTS 版本）
- npm

## 本地运行

```bash
# 1. 安装依赖
npm install

# 2. 启动开发服务器（默认 http://localhost:5173）
npm run dev
```

### 可用命令

```bash
npm run dev      # 启动 Vite 开发服务器（支持热更新）
npm run build    # 类型检查 + 打包产物到 dist/
npm run preview  # 本地预览生产构建产物
npm run lint     # 运行 oxlint 代码检查
```

### 局域网 / 外部访问

开发服务器与预览服务已配置为监听 `0.0.0.0`，同一局域网内的手机、平板或其他电脑可以直接通过本机 IP 访问，方便真机调试：

```bash
# 终端会打印形如 http://192.168.x.x:5173 的 Network 地址，手机浏览器直接打开即可
npm run dev
```

端口可通过 `PORT` 环境变量覆盖，默认开发端口为 `5173`，预览端口为 `4173`：

```bash
PORT=8080 npm run dev
```

> 若只想在本机访问，可把 `vite.config.ts` 中的 `host: '0.0.0.0'` 改回默认值。

## 页面结构

| 路由 | 说明 |
| --- | --- |
| `/` | 首页 —— Hero、技术溯源时间轴、产品展示、运动场景 |
| `/product` | 产品页 —— 产品洞察、特性、竞品对比、产品展示 |
| `/training` | 培训页 —— 培训卡片 |
| `/faq` | 常见问题 —— 12 个分类、共 70 条问答 |

## 目录结构

```
recovery-plus/
├── public/
│   └── assets/            # 图片资源，直接以 /assets/xxx 引用
├── src/
│   ├── main.tsx           # 应用入口
│   ├── App.tsx            # 路由配置（/、/product、/training、/faq）
│   ├── index.css          # Tailwind 指令 + CSS Reset + 自定义 @keyframes
│   ├── types/             # TypeScript 接口定义
│   ├── data/              # 全站文案内容（见下方说明）
│   ├── hooks/             # useStickyNav / useScrollSpy / useMobileMenu
│   ├── components/
│   │   ├── layout/        # Layout、Nav、Footer
│   │   ├── ui/            # Button、Card、SectionHeader、Reveal、Icons 等
│   │   ├── sections/      # 首页与产品页各区块组件
│   │   └── faq/           # FAQ 相关组件
│   └── pages/             # HomePage / ProductPage / TrainingPage / FaqPage
├── tailwind.config.ts     # 设计令牌（颜色、间距、字号、圆角、阴影）
├── vite.config.ts         # 构建与开发服务器配置
└── CLAUDE.md              # 项目架构与开发约定说明
```

## 内容维护

**所有文案、产品参数、对比项、FAQ 问答都写在 `src/data/` 下的 TypeScript 文件中**，组件本身只负责渲染：

```
src/data/
├── hero.ts              # 首屏标题、标签、按钮
├── timeline.ts          # 技术溯源时间轴节点
├── timeline-history.ts  # 时间轴历史图片
├── features.ts          # 特性卡片
├── comparison.ts        # 竞品对比表
├── products.ts          # 产品卡片与规格
├── scenes.ts            # 运动场景
├── trust.ts             # 数据指标与信任背书
├── training.ts          # 培训卡片
└── faq.ts               # 12 个分类 × 70 条问答
```

> 需要改文案时，请直接修改对应的数据文件，不要动组件。

## 设计令牌

原始 CSS 的 `:root` 变量已全部映射到 `tailwind.config.ts` 的 `theme.extend`：

- **颜色**：`bg-primary`、`bg-card`、`text-secondary`、`brand`、`border`、`accent`、`danger`、`success` —— 使用方式如 `bg-bg-primary`、`text-text-secondary`
- **间距**：语义化键名 `xs`–`5xl` —— 使用方式如 `p-md`、`gap-2xl`
- **字体**：`font-sans`（Noto Sans SC + Inter）、`text-display`（首屏自适应字号）
- **圆角 / 阴影**：`rounded-sm`–`rounded-full`、`shadow-sm/md/lg`

缓动曲线（`--ease-out`、`--ease-in-out`）以 CSS 变量形式定义在 `src/index.css` 的 `@layer base` 中。

## 开发注意事项

- **FAQ 折叠面板**使用 React `useState` 控制显隐，**没有**使用原生 `<details>/<summary>`（原生方案在本项目跨浏览器表现不稳定）。
- **锚点跳转**：首页内使用 `<a href="/#sectionId">` 交由浏览器原生滚动；跨页面跳转时通过路由 state 传递目标区块，`HomePage` 挂载后再执行滚动。**请勿使用 React Router 的 `<Link to="/#hash">`**，会触发页面空白的问题。
- **移动端菜单**：不要在 Nav 链接的 `onClick` 中调用 `useMobileMenu` 的 `close()`，会在点击事件处理过程中触发重渲染，破坏 React Router 的事件委托。
- **图片资源**放在 `public/assets/`，通过 `/assets/...` 引用。项目根目录下的 `assets/` 目录**不会**被 Vite 提供访问。

## 浏览器支持

支持 Chrome、Edge、Firefox、Safari 的最新两个版本。项目中使用了 `backdrop-filter`、`aspect-ratio`、`scroll-snap` 与 CSS 变量，现代浏览器均已支持。
