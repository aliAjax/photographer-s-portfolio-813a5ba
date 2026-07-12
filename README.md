# 摄影师作品集 | Photographer's Portfolio

一个使用 React、TypeScript 和 Vite 构建的独立摄影师作品集网站。项目围绕黑白人像、高原风光与牧场生活三个系列展开，通过沉浸式图片排版、跨页面 Lightbox 和响应式交互呈现摄影作品。

完整产品需求与验收约束见 [`task.md`](task.md)。

## 功能

- 首页 `/`：摄影师简介与三个系列的精选入口
- 作品集 `/work`：全部照片网格，以及肖像、风光、牧野分类筛选
- 系列详情 `/work/:seriesId`：基于同一份照片数据的叙事式长图文页面
- 关于 `/about`：摄影师简介与经历时间线
- 联系 `/contact`：带行内校验、提交状态和成功反馈的联系表单
- 全局 Lightbox：在多个页面复用，支持关闭、前后导航和照片说明
- 响应式布局：桌面端多列作品网格与移动端单列布局
- 离线字体：所有字体均通过本地 `@font-face` 加载

## 技术栈

- React
- TypeScript
- Vite
- 客户端路由
- Playwright
- 本地 JSON 数据与静态图片资源

## 项目目录

根目录就是可运行的前端工程：

```text
Photographer's-Portfolio/
├── index.html                     # Vite HTML 入口
├── package.json                   # 应用依赖与 npm scripts
├── package-lock.json
├── tsconfig.json                  # TypeScript 配置
├── vite.config.ts                 # Vite 配置
├── src/                           # React 应用源码
├── public/                        # 运行时静态资源
├── mock-data/                     # 权威业务数据
│   ├── photos.json                # 分类、系列、文案与图片尺寸
│   └── photos/
│       ├── portrait/              # 肖像，5 张
│       ├── landscape/             # 风光，5 张
│       └── pastoral/              # 牧野，4 张
├── assets/                        # 视觉参考与离线字体源文件
│   ├── fonts/
│   ├── FONTS.md
│   └── reference_*.png
├── tests/                         # Playwright 验收测试
│   ├── e2e/
│   ├── helpers/
│   └── playwright.config.ts
├── task.md                        # 产品需求与实现约束
└── README.md                      # 项目说明
```

`src/` 和 `public/` 的具体内容以工程实现为准。应用代码、构建配置和依赖清单均位于根目录，根目录是唯一运行入口。

## 数据与素材

`mock-data/photos.json` 是照片内容的唯一权威数据源，包含 14 张照片的分类、系列、标题、说明和真实宽高。作品集、系列详情和 Lightbox 应复用这份数据，不得重复硬编码或篡改业务内容。

真实照片位于 `mock-data/photos/<category>/`。`assets/` 只提供设计参考图、字体规范和字体源文件，不能把 `reference_*.png` 当作作品照片。

字体必须由本地文件提供，禁止请求 Google Fonts 或其他外部字体 CDN。

## 安装与运行

在仓库根目录执行：

```bash
npm install
npm run dev
```

开发服务器默认由 Vite 启动。生产构建验证：

```bash
npm run build
```

根目录 `package.json` 必须至少提供 `dev` 和 `build` scripts。

## 自动化测试

先安装根目录应用依赖，再安装 Playwright 测试依赖：

```bash
npm install

cd tests
npm install
npx playwright install chromium   # 首次运行需要
npm test
```

Playwright 会在 `127.0.0.1:5173` 自动启动根目录应用，无需提前手动运行开发服务器。

其他测试命令：

```bash
cd tests
npm run test:headed               # 有头模式
npm run report                    # 查看上一次 HTML 报告
```

视觉还原和整体观感仍需结合 `assets/reference_*.png`、`target_states.md` 与 `screenshots/` 人工复核。

## 评测与辅助资料

- `metadata.json`：任务元数据、难度和技术栈说明
- `rubric.json`：机器评分规则
- `target_states.md`：验收状态 A-F 的操作与预期
- `trajectory.jsonl`：coding agent 的完整执行轨迹
- `screenshots/`：已有验收状态截图
- `starter/`：保留的历史占位目录，不参与安装、构建、运行或测试

## 关键约束

- `/work` 的分类筛选状态需要在详情页往返时保持。
- Lightbox 的导航范围必须跟随当前筛选结果。
- 图片加载前必须根据真实宽高预留比例，避免布局抖动。
- 系列详情与作品集必须共享 `mock-data/photos.json` 数据模型。
- 移动端需要切换为单列网格和底部 Lightbox 信息条。
- 联系表单提交仅在前端模拟，不会真实发送邮件或持久化数据。
