# 摄影师作品集 Showcase 任务包

这是一个面向 coding agent 的高难度前端评测任务。任务要求使用 React、TypeScript 和 Vite，从零实现一个独立摄影师作品集网站，并通过真实内容数据、视觉参考图和自动化测试验证多项相互耦合的交互约束。

本仓库保存的是**任务定义、素材与验收工具**，不是已经实现完成的网站。当前仓库根目录没有可直接运行的前端应用，参评实现需要单独放入指定目录后再启动或验收。

## 目标技术栈

- React + TypeScript + Vite
- 5 个客户端路由：首页、作品集、系列详情、关于和联系
- 1 个跨页面复用的全局 Lightbox
- 本地静态数据与自托管字体，无后端服务
- Playwright 浏览器验收

完整功能要求见 [`task.md`](task.md)，评分规则见 [`rubric.json`](rubric.json)。

## 项目目录

下面的目录树以当前 Git 仓库实际跟踪的内容为准：

```text
Photographer's-Portfolio/
├── README.md                     # 项目说明
├── task.md                       # 交给 coding agent 的实现需求
├── metadata.json                 # 任务元数据、难度与技术栈信息
├── rubric.json                   # 机器评分规则
├── target_states.md              # 验收状态 A-F 的操作与预期
├── assets/                       # 视觉参考和离线字体，不是业务照片
│   ├── README.md
│   ├── FONTS.md
│   ├── fonts/
│   │   ├── inter-400-600.woff2
│   │   ├── playfair-display-600.woff2
│   │   └── playfair-display-italic-400.woff2
│   ├── reference_home_desktop.png
│   ├── reference_work_desktop.png
│   ├── reference_work_mobile.png
│   ├── reference_series_desktop.png
│   ├── reference_lightbox_desktop.png
│   └── reference_about_desktop.png
├── mock-data/                    # 权威业务数据，参评实现不得篡改
│   ├── photos.json               # 照片、分类、系列和尺寸元数据
│   └── photos/
│       ├── portrait/             # 肖像，5 张
│       ├── landscape/            # 风光，5 张
│       └── pastoral/             # 牧野，4 张
├── tests/                        # annotator 使用的 Playwright 验收套件
│   ├── README.md
│   ├── package.json
│   ├── package-lock.json
│   ├── playwright.config.ts
│   ├── tsconfig.json
│   ├── helpers/
│   │   ├── photoData.ts
│   │   └── lightbox.ts
│   └── e2e/
│       ├── structural-baseline.spec.ts
│       ├── content-integrity.spec.ts
│       └── coupled-constraints.spec.ts
└── screenshots/                  # 已有验收状态截图
    ├── sota_state_a.png
    ├── sota_state_b.png
    ├── sota_state_c1.png
    ├── sota_state_c2.png
    ├── sota_state_d.png
    ├── sota_state_e.png
    ├── sota_state_f1.png
    └── sota_state_f2.png
```

### 未包含在仓库中的工作目录

- `starter/`：当前本地存在但为空。Git 不跟踪空目录，因此 GitHub 仓库中不会显示它。若以 `starter/` 作为参评工作区，需要先在其中创建 Vite 项目。
- `sota-submission/`：`tests/playwright.config.ts` 当前固定以该目录作为被测应用目录，但它没有提交到本仓库。运行验收测试前，需要在仓库根目录下准备一份完整、可执行 `npm run dev` 的实现。
- `reference-solution/`：本仓库未包含参考答案。
- `trajectory.jsonl`、`.DS_Store`、依赖目录和测试产物：由根目录 `.gitignore` 排除。

## 核心任务

参评实现需要完成以下页面和共享能力：

- `/`：摄影师简介和三个系列的精选入口
- `/work`：全部照片网格及肖像、风光、牧野筛选
- `/work/:seriesId`：复用同一数据源的系列叙事详情
- `/about`：个人简介和经历时间线
- `/contact`：带校验、提交中和成功反馈的联系表单
- 全局 Lightbox：支持打开、关闭、前后导航及照片说明

重点约束包括筛选状态跨导航保持、Lightbox 导航范围跟随当前筛选结果、按真实尺寸预留图片比例、移动端布局切换、离线字体，以及完整的表单状态。详细定义和禁止事项以 [`task.md`](task.md) 为准。

## 数据与素材

`mock-data/photos.json` 是业务内容的唯一权威数据源，包含 14 张照片的分类、系列、标题、说明和真实宽高。照片文件位于 `mock-data/photos/<category>/`，不得臆造、替换或删减。

`assets/` 只提供设计参考图、字体规范和字体文件。实现时不得把 `reference_*.png` 当作作品照片，也不得从 Google Fonts 等外部 CDN 加载字体。

## 文件可见性

将任务交给参评 agent 时，应避免暴露评分标准和验收结果：

| 文件或目录 | 建议可见性 | 用途 |
|---|---|---|
| `task.md` | agent 可见 | 实现需求 |
| `mock-data/` | agent 可见 | 真实内容数据 |
| `assets/` | agent 可见 | 视觉参考和本地字体 |
| `README.md` | agent 可见 | 仓库结构说明 |
| `metadata.json` | 仅 annotator | 难度与评测元数据 |
| `rubric.json` | 仅 annotator | 评分规则 |
| `target_states.md` | 仅 annotator | 详细验收操作与预期 |
| `tests/` | 仅 annotator | 自动化验收脚本 |
| `screenshots/` | 仅 annotator | 已有验收结果截图 |

建议为参评 agent 准备隔离工作区，只复制 `task.md`、`mock-data/` 和 `assets/`，再在该工作区中创建前端工程。

## 运行参评实现

仓库本身没有根级 `package.json`，不能直接在根目录执行 `npm install` 或 `npm run dev`。创建或放入参评实现后，在实现目录中运行：

```bash
npm install
npm run dev
npm run build
```

实现项目必须至少提供 `dev` 和 `build` 两个 npm script。

## 运行验收测试

当前 Playwright 配置会从 `sota-submission/` 启动被测应用。目录布局应为：

```text
Photographer's-Portfolio/
├── sota-submission/              # 完整的参评前端实现，不纳入本仓库
└── tests/
```

先安装参评应用依赖，再安装并运行测试：

```bash
cd sota-submission
npm install

cd ../tests
npm install
npx playwright install chromium   # 首次运行需要
npm test
```

其他测试命令：

```bash
npm run test:headed               # 有头模式
npm run report                    # 查看上一次 HTML 报告
```

测试配置会在 `127.0.0.1:5173` 自动启动 `sota-submission`。视觉还原和整体观感仍需结合 `assets/reference_*.png`、`target_states.md` 与 `screenshots/` 人工复核。

## 已知限制

- 联系表单只要求前端模拟提交，不会真实发送邮件或持久化数据。
- 当前仓库不包含可运行的网站实现，因此未准备 `sota-submission/` 时，Playwright 测试无法启动被测服务。
