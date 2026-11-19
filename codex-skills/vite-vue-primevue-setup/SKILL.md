---
name: vite-vue-primevue-setup
description: Use only when the user explicitly requests “请配置前端” to set up the Vite, Vue 3, TypeScript and PrimeVue v4 frontend baseline.
---

# Vite + Vue + PrimeVue 前端环境

## When To Use

只有当用户明确说出“请配置前端”并以此要求执行前端配置时进入本流程；单纯引用、讨论或否定这句话不触发。其他前端开发或单项配置请求不自动执行本 skill，也不自动补装整套基线。

收到该请求后，首先询问用户是否执行 `vite-vue-primevue-setup` 这套前端配置 skill，并等待明确确认；确认前不开始下方检查、安装或配置。用户拒绝时不执行；答复不明确时继续询问。已有的目录、版本等选择继续有效，执行确认不自动授权更新 Node 或安装可选 Chart。

## 开始之前

1. 阅读目标项目的规则、现有文件和工作区状态；确定新建项目还是补充现有项目。检查包管理器、锁文件、脚本和 Node 的安装方式，不预设目录名、包管理器或专用 wrapper。
2. 确认前端所在位置和文件夹名；用户尚未明确时询问，已明确的不重复确认。使用现有项目时不得覆盖已有文件。对既有配置，展示必要变更并尊重用户已有选择；不为强行统一而覆盖或格式化无关内容。
3. 检查当前 Node 版本并对照官方最新 LTS；不同版本时说明差异并询问用户是否更新，不自动升级或降级。用户拒绝时检查当前版本是否满足目标依赖，兼容则继续，否则暂停依赖该环境的安装。先确认 npm 或其他包管理器与 Node 兼容，不自动修改全局环境。
4. 安装前核对直接依赖、相关传递依赖及字体具体版本的来源、许可和商业发行条件，记录需保留的版权及许可。许可不明、有限制或收费时，先向用户说明可选替代或旧版并取得明确选择；无法核实时标记待核实，不声称已合规。
5. PrimeVue 限定已核实 MIT 的 v4 最新稳定版本，PrimeIcons 固定 `7.0.0`；PrimeVue Forms 选择与该 v4 兼容、许可已核实且满足条件的最新稳定版本。其他依赖以当时最新稳定版为候选，同时核查 Node、peer dependencies 和版本兼容性；若不能兼容，说明冲突并与用户确定方案，不强行安装或静默降级。

## 配置基线

按 [基础配置](references/baseline.md) 分步建立并核对 Vite、Vue 3、TypeScript、Vue Router、Pinia、Vue I18n、Vitest、ESLint、Prettier、PrimeVue v4、PrimeVue Forms、PrimeIcons、Tailwind CSS、`tailwindcss-primeui` 和本地 Roboto。使用 [Prettier 默认配置](assets/prettierrc.json)作为新项目基线；现有配置不直接覆盖。

按 [主题基线](references/theme.md) 提供 Aura、天空蓝主色、跟随系统的初始明暗模式、独立明暗切换、主色及 Surface 选择和颜色重置；不实现个性化持久化，不依赖特定项目的数据库。尊重目标项目对 PrimeVue 样式和组件行为的确认要求。

询问是否需要 PrimeVue Chart；若需要，先按 [图表说明](references/chart.md) 核实依赖、版本及许可并取得用户确认，再安装。未选择时不安装。

使用官方文档及目标项目实际安装版本核查配置，不能访问时说明限制并结合已安装包核对，不推测 API。不要一次生成业务模块、布局或预置业务文案。

## 验证与交付

- 使用目标项目的命令验证类型检查、构建、ESLint、Prettier 和 Vitest；配置命令时确保它们真实可运行。检查 Router、Pinia、三语、PrimeVue Forms、Icons、主题和样式集成。
- 为路由、主题切换及表单等准备最小验证方式，验证后清理临时演示内容或明确说明保留内容。浏览器不可用时如实报告，并给出用户可操作的验收步骤，不将构建通过当作视觉测试通过。
- 报告前端位置、选择的版本与许可核查范围、已执行检查及结果、未验证事项；若涉及依赖或配置变更，遵守目标项目的文档与交付规则。
