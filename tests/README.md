# tests/ - 标准化验收测试套件

这套 Playwright 测试对应 `rubric.json` 中需要浏览器验证的判定项，用于重复验证根目录摄影师作品集应用的路由、内容完整性、共享组件和耦合约束。

测试直接启动仓库根目录应用。根目录必须包含完整工程，并在 `package.json` 中提供 `dev` script。

## 用法

先在仓库根目录安装应用依赖：

```bash
npm install
```

再安装并运行测试：

```bash
cd tests
npm install
npx playwright install chromium   # 首次运行需要
npm test                          # 无头运行全部用例
npm run test:headed               # 有头模式，便于调试
npm run report                    # 查看上一次运行的 HTML 报告
```

`playwright.config.ts` 会从仓库根目录执行：

```bash
npm run dev -- --port 5173 --host 127.0.0.1
```

测试套件会等待 `http://127.0.0.1:5173` 可用，并在测试结束后关闭其启动的开发服务器。若该端口已有可复用的服务，则直接使用现有服务。

## 目录结构

```text
tests/
├── playwright.config.ts          # baseURL、根目录 web server 和全局浏览器配置
├── helpers/
│   ├── photoData.ts              # 从 mock-data/photos.json 派生期望结果
│   └── lightbox.ts               # Lightbox 选择器与交互封装
└── e2e/
    ├── structural-baseline.spec.ts   # 路由、共享 Lightbox 和筛选 UI
    ├── content-integrity.spec.ts     # 照片数据一致性与资源隔离
    └── coupled-constraints.spec.ts   # task.md 中的耦合约束
```

## 覆盖范围

- 五个路由能够正确渲染。
- `/work` 存在分类筛选，并保持导航前后的筛选状态。
- Lightbox 在首页、作品集和系列详情三个入口复用同一套行为。
- 筛选后的 Lightbox 只在当前结果范围内导航。
- 页面内容与 `mock-data/photos.json` 保持一致。
- 图片按数据中的真实宽高预留比例。
- 移动端布局与 Lightbox 信息栏正确切换。
- 页面不加载外部字体 CDN。
- 联系表单具有校验、提交中和成功反馈状态。

`visual_fidelity` 和 `overall_impression` 对应截图审查、LLM 判断或人工复核，不由本套自动化测试完全覆盖。相关检查需结合 `assets/reference_*.png`、`target_states.md` 和 `screenshots/` 完成。

## 共享 Lightbox 用例

`structural-baseline.spec.ts` 会依次从 `/work` 网格、系列详情页和首页精选预览图打开 Lightbox。三个入口都必须使用全局共享的 Lightbox 行为；任一入口仅执行普通页面跳转或使用独立实现，都应被视为测试失败。
