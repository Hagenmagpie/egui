# 架构
本文档描述了组成 egui 的 crate 是如何连接的。

还请参阅 [`CONTRIBUTING.md`](CONTRIBUTING.md) 了解在开放 PR 之前应该做什么。

## Crate 概述
本仓库中的 crate 包括：`egui, emath, epaint, egui_extras, egui_plot, egui-winit, egui_glow, egui_demo_lib, egui_demo_app`。

### `egui`: 主 GUI 库。
示例代码：`if ui.button("Click me").clicked() { … }`
这是大部分代码所在的 crate。`egui` 仅依赖于 `emath` 和 `epaint`。

### `emath`: 最小的 2D 数学库
示例：`Vec2, Pos2, Rect, lerp, remap`

### `epaint`
可以转换为纹理三角形的 2D 形状和文本。

示例：`Shape::Circle { center, radius, fill, stroke }`

依赖于 `emath`。

### `egui_extras`
在 `egui` 之上添加附加功能。

### `egui_plot`
用于 `egui` 的绘图。

### `egui-winit`
这个 crate 提供了 [`egui`](https://github.com/emilk/egui) 和 [winit](https://crates.io/crates/winit) 之间的绑定。

该库将 winit 事件转换为 egui，处理复制/粘贴，更新光标，打开在 egui 中点击的链接等。

### `egui_glow`
将 egui 应用程序放入笔记本电脑上的本机窗口中。使用 [glow](https://github.com/grovesNL/glow) 绘制 egui 输出的三角形。

### `eframe`
`eframe` 是官方的 `egui` 框架，可以为 Web 或本地编译相同的应用程序。

您可以在 <https://www.egui.rs> 上看到的演示使用 `eframe` 托管了 `egui`。演示代码位于：

### `egui_demo_lib`
依赖于 `egui`。
其中包含了大量使用 `egui` 的用例，并且看起来像是为 `egui` 应用程序编写的 UI 代码。

### `egui_demo_app`
`egui_demo_lib` 的薄包装，以便我们可以将其编译为网站或本地应用程序可执行文件。
依赖于 `egui_demo_lib` + `eframe`。

### 其他集成

还有许多针对游戏引擎（如 `bevy` 和 `miniquad`）的出色集成，您可以在 <https://github.com/emilk/egui#integrations> 找到。
