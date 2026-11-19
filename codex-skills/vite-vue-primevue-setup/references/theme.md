# 主题基线

本参考记录所需行为与默认值，不依赖任何特定项目的状态库、路由、账本或持久化接口。实现时核实当前 PrimeVue v4 和 Tailwind API，并遵守目标项目的组件样式规则。

## 默认值与状态

- 使用 PrimeVue v4 Aura styled preset，默认主色 `sky`，完整映射 50–950 色阶。PrimeVue 使用 `.app-dark` 控制深色模式，Tailwind 的 `dark:` 与同一选择器对齐。
- 每次启动使用默认颜色，不读取或写入 localStorage、sessionStorage、数据库或其他用户偏好。初始明暗模式跟随系统，手动切换前响应系统变化；手动切换后当前会话使用用户选择。
- 默认 surface 在浅色模式为 `slate`、深色模式为 `zinc`；选中自定义 surface 后保持选择。重置颜色恢复 `sky` 和随明暗模式变化的默认 surface，不改变当前明暗模式。
- 状态和主题更新逻辑与视图分离，允许按项目使用的 Vue/Pinia 组织方式实现；不得引入原参考项目的账本、Toast 或网络调用。

## 可选主色和 Surface

- 主色：`noir`, `emerald`, `green`, `lime`, `orange`, `amber`, `yellow`, `teal`, `cyan`, `sky`, `blue`, `indigo`, `violet`, `purple`, `fuchsia`, `pink`, `rose`。
- Surface：`slate`, `gray`, `zinc`, `neutral`, `stone`, `soho`, `viva`, `ocean`。
- 常规颜色使用 PrimeVue 对应色阶；`noir` 使用 surface 色阶及适用于浅色和深色的 primary/highlight 对比色。`soho`、`viva`、`ocean` 若不由当前主题提供，应在核验来源及许可后使用完整 0–950 色阶，不能只设置单个色块。

参照基线，`noir` 的浅色模式 primary 使用 `primary.950`，hover `primary.800`、active `primary.700`、对比文字白色；深色模式使用 `primary.50`，hover `primary.200`、active `primary.300`、对比文字 `primary.950`。浅色 highlight 背景 `primary.950`、焦点背景 `primary.700`、文字白色；深色 highlight 背景 `primary.50`、焦点背景 `primary.300`、文字 `primary.950`。从 `noir` 切回普通颜色时同步恢复普通颜色的浅深模式 primary 和 highlight 令牌，不得保留黑白语义配置。

以下为参照基线的 Surface 色阶，顺序为 `0, 50, 100, 200, 300, 400, 500, 600, 700, 800, 900, 950`：

- `soho`: `#ffffff`, `#ececec`, `#dedfdf`, `#c4c4c6`, `#adaeb0`, `#97979b`, `#7f8084`, `#6a6b70`, `#55565b`, `#3f4046`, `#2c2c34`, `#16161d`。
- `viva`: `#ffffff`, `#f3f3f3`, `#e7e7e8`, `#cfd0d0`, `#b7b8b9`, `#9fa1a1`, `#87898a`, `#6e7173`, `#565a5b`, `#3e4244`, `#262b2c`, `#0e1315`。
- `ocean`: `#ffffff`, `#fbfcfc`, `#f7f9f8`, `#eff3f2`, `#dadedd`, `#b1b7b6`, `#828787`, `#5f7274`, `#415b61`, `#29444e`, `#183240`, `#0c1920`。

参照 [PrimeVue v4 Styled Mode](https://v4.primevue.org/theming/styled/) 核实 `definePreset`、`updatePreset`、`updateSurfacePalette` 及实际安装版本的主题令牌；核实第三方代码或资源的来源和许可，不自动把外部示例视为可复制资产。

## 交互与验证

- 提供单独的明暗切换按钮及主题设置入口；主题设置中展示主色、Surface 色板和重置按钮。使用 PrimeVue 组件及公开接口，并遵守项目的样式确认规则；有 `small` 且语义适用的组件使用 `size="small"`。
- 交互状态包括可辨认的选中色、可访问名称、键盘焦点和禁用状态；德语、英语、中文的用户界面文案进入翻译资源。
- 验证明暗模式随系统初始化和手动切换；更换各类主色（包含 `noir`）、Surface、重置和刷新后的默认值。实际浏览器不可用时列出待用户验收的步骤，不把代码测试当作视觉验收。
