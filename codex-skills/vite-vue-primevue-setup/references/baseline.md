# 前端基础配置

本参考适用于完整基线；用户只要求部分功能时，按范围选用。版本、项目名称、目录、包管理器和配置方式根据目标项目确定，不能照抄旧项目的路径或数据库实现。

## 创建和工具

- 选择兼容最新 LTS 的 Vue 3 + TypeScript 脚手架，避免重复初始化 Git。使用目标项目采用的包管理器及锁文件；没有现成项目时，与用户确认目录后再创建。
- 接入 Vue Router（含根路由）、Pinia、Vue I18n 和 Vitest。国际化提供德语、英语、中文的独立资源文件；默认德语，回退英语。根据功能分组资源，并确保文本、页面标题及语言切换按项目需要接入。
- 新项目采用本 skill 的 [Prettier 配置](../assets/prettierrc.json)。按当前 Vue/TypeScript 和 ESLint 版本配置兼容的 flat config，结合 Prettier 避免规则冲突。现有项目保留其约定，必要变更先说明。
- 至少提供目标项目可执行的开发、构建、类型检查、lint、格式检查和 Vitest 命令；不要把已有命令当作未经验证的新命令。版本和依赖从项目 lockfile及实际安装结果核对。

## PrimeVue v4 与 Tailwind

- 采用许可核实后的 PrimeVue v4、匹配版本的 `@primevue/forms` 和兼容的 `@primeuix/themes`；安装并导入固定版 `primeicons@7.0.0` 的 CSS。按 PrimeVue v4 [Configuration](https://v4.primevue.org/configuration/) 注册插件；仅按需要配置公开选项。表单使用 PrimeVue Forms，检查与实际包版本一致的 API。
- 按 v4 [Tailwind CSS](https://v4.primevue.org/tailwind/) 文档和安装版本接入 `tailwindcss`、`@tailwindcss/vite`、`tailwindcss-primeui`。FWM 使用 Tailwind 4 的 Vite 插件、`@import 'tailwindcss'` 和 `@import 'tailwindcss-primeui'`；若最新版文档或兼容性要求不同，先核对再调整。
- 启用 PrimeVue CSS layer 时，确保层的声明和顺序与 Tailwind 一致。FWM 的顺序为 `theme, base, primevue, components, utilities`。PrimeVue 与 Tailwind dark 变体使用同一个 `.app-dark` 选择器；验证主题、surface、浅色和深色工具类。
- 如需更改组件已有功能或样式，遵守目标项目的确认规则；不要把使用此 skill 视为对全部样式覆盖的授权。不要默认配置组件自动导入或额外的全局 Pass Through。

## Roboto

- 使用本地 Roboto，不依赖运行时在线字体服务。核对具体文件来源及许可，保留相应许可与版权声明；只引入项目需要的正常和斜体字重或可变字体。
- 通过 `@font-face` 在应用全局样式中引入，并与 Tailwind 字体设置一致。路径按目标项目的静态资源规则确定，验证构建及部署路径；不假设固定使用网站根路径。

## 官方参考

- [Vite](https://vite.dev/guide/)、[Vue](https://vuejs.org/guide/)、[Vue Router](https://router.vuejs.org/)、[Pinia](https://pinia.vuejs.org/)、[Vue I18n](https://vue-i18n.intlify.dev/)、[Vitest](https://vitest.dev/)
- [PrimeVue v4 Configuration](https://v4.primevue.org/configuration/)、[Forms](https://v4.primevue.org/forms/)、[Icons](https://v4.primevue.org/icons/)、[Tailwind](https://v4.primevue.org/tailwind/)
- [Node.js releases](https://nodejs.org/en/about/previous-releases)、[Roboto 源码和许可](https://github.com/googlefonts/roboto)

安装前以实际版本的来源、许可和兼容要求为准；文档不可读时说明，不能假装已核验。
