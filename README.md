# 🖌 egui: an easy-to-use GUI in pure Rust

[<img alt="github" src="https://img.shields.io/badge/github-emilk/egui-8da0cb?logo=github" height="20">](https://github.com/emilk/egui)
[![Latest version](https://img.shields.io/crates/v/egui.svg)](https://crates.io/crates/egui)
[![Documentation](https://docs.rs/egui/badge.svg)](https://docs.rs/egui)
[![unsafe forbidden](https://img.shields.io/badge/unsafe-forbidden-success.svg)](https://github.com/rust-secure-code/safety-dance/)
[![Build Status](https://github.com/emilk/egui/workflows/CI/badge.svg)](https://github.com/emilk/egui/actions?workflow=CI)
[![MIT](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/emilk/egui/blob/master/LICENSE-MIT)
[![Apache](https://img.shields.io/badge/license-Apache-blue.svg)](https://github.com/emilk/egui/blob/master/LICENSE-APACHE)
[![Discord](https://img.shields.io/discord/900275882684477440?label=egui%20discord)](https://discord.gg/JFcEma9bJq)

<div align="center">
<a href="https://www.rerun.io/"><img src="media/rerun_io_logo.png" width="250"></a>

egui 的开发由 [Rerun](https://www.rerun.io/) 赞助，这是一家正在构建<br>
用于可视化多模态数据流的 SDK 的初创公司。
</div>

---

👉 [点击运行 Web 演示](https://www.egui.rs/#demo) 👈

egui（发音为 "e-gooey"）是一个简单、快速且高度可移植的 Rust 即时模式 GUI 库。egui 可在 Web、本地和[您喜爱的游戏引擎中运行](#integrations)。

egui 的目标是成为最易于使用的 Rust GUI 库，以及使用 Rust 制作 Web 应用程序的最简单方法。

egui 可以在任何您可以绘制纹理三角形的地方使用，这意味着您可以轻松地将其集成到您选择的游戏引擎中。

[`eframe`](https://github.com/emilk/egui/tree/master/crates/eframe) 是官方的 egui 框架，支持编写适用于 Web、Linux、Mac、Windows 和 Android 的应用程序。



## Example

``` rust
ui.heading("My egui Application");
ui.horizontal(|ui| {
    ui.label("Your name: ");
    ui.text_edit_singleline(&mut name);
});
ui.add(egui::Slider::new(&mut age, 0..=120).text("age"));
if ui.button("Increment").clicked() {
    age += 1;
}
ui.label(format!("Hello '{name}', age {age}"));
ui.image(egui::include_image!("ferris.png"));
```

<img alt="Dark mode" src="media/demo.gif"> &nbsp; &nbsp; <img alt="Light mode" src="media/demo_light_mode.png" height="278">

## 章节：

* [Example](#example)
* [Quick start](#quick-start)
* [Demo](#demo)
* [Goals](#goals)
* [State / features](#state)
* [Dependencies](#dependencies)
* [Who is egui for?](#who-is-egui-for)
* [Integrations](#integrations)
* [Why immediate mode](#why-immediate-mode)
* [FAQ](#faq)
* [Other](#other)
* [Credits](#credits)

([egui 的中文翻译文档 / chinese translation](https://github.com/Re-Ch-Love/egui-doc-cn/blob/main/README_zh-hans.md))


## 快速入门

在 [examples/ 文件夹](https://github.com/emilk/egui/blob/master/examples/)中有简单的示例。如果您想编写一个 Web 应用程序，请访问 <https://github.com/emilk/eframe_template/> 并按照说明操作。官方文档位于 <https://docs.rs/egui>。要获取灵感和更多示例，请查看 [egui web 演示](https://www.egui.rs/#demo) 并按照其中的链接查看其源代码。

如果您想将 egui 集成到现有的引擎中，请转到 [Integrations](#integrations) 章节。

如果您有问题，请使用 [GitHub Discussions](https://github.com/emilk/egui/discussions)。还有 [一个 egui Discord 服务器](https://discord.gg/JFcEma9bJq)。如果您想为 egui 做贡献，请阅读 [贡献指南](https://github.com/emilk/egui/blob/master/CONTRIBUTING.md)。


## Demo

[点击运行 egui web 演示](https://www.egui.rs/#demo)（适用于具有 Wasm 和 WebGL 支持的任何浏览器）。使用 [`eframe`](https://github.com/emilk/egui/tree/master/crates/eframe)。

要在本地测试演示应用程序，请运行 `cargo run --release -p egui_demo_app`。

本机后端是 [`egui_glow`](https://github.com/emilk/egui/tree/master/crates/egui_glow)（使用 [`glow`](https://crates.io/crates/glow)），应该可以在 Mac 和 Windows 上直接使用，但在 Linux 上您需要先运行：

`sudo apt-get install -y libclang-dev libgtk-3-dev libxcb-render0-dev libxcb-shape0-dev libxcb-xfixes0-dev libxkbcommon-dev libssl-dev`

在 Fedora Rawhide 上，您需要运行：

`dnf install clang clang-devel clang-tools-extra libxkbcommon-devel pkg-config openssl-devel libxcb-devel gtk3-devel atk fontconfig-devel`

**注意**：这仅适用于演示应用程序 - egui 本身完全与平台无关！

## 目标

* 最易于使用的 GUI 库
* 响应式：在调试构建中目标为 60 Hz
* 用户友好：难以犯错，不应该出现 panic
* 可移植：相同的代码可以在 Web 和本机应用程序上运行
* 易于集成到任何环境中
* 用于自定义绘制的简单的 2D 图形 API（[`epaint`](https://docs.rs/epaint)）
* 无回调
* 纯即时模式
* 可扩展：[轻松编写自己的 egui 小部件](https://github.com/emilk/egui/blob/master/crates/egui_demo_lib/src/demo/toggle_switch.rs)
* 模块化：您应该能够使用 egui 的小部分，并以新的方式组合它们
* 安全：egui 中没有 `unsafe` 代码
* 最小的依赖关系

egui 不是一个框架。egui 是一个你调用的库，而不是一个你为之编程的环境。

**注意**：egui 尚未达到所有这些目标！egui 仍然在积极开发中。

### 非目标

* 成为最强大的 GUI 库
* 本机外观界面
* 高级和灵活的布局（这与即时模式基本不兼容）

## 状态

egui 正在积极开发中。它很好地完成了它的任务，但它缺乏许多功能，并且接口仍在变化中。新版本将具有破坏性变更。

尽管如此，egui 可用于创建专业的应用程序，如 [Rerun Viewer](https://app.rerun.io/)。

### 特性

* 小部件：标签、文本按钮、超链接、复选框、单选按钮、滑块、可拖动的值、文本编辑、颜色选择器、微调器
* 图像
* 布局：水平、垂直、列、自动换行
* 文本编辑：多行、复制/粘贴、撤销、支持表情符号
* 窗口：移动、调整大小、命名、最小化和关闭。自动调整大小和定位。
* 区域：调整大小、垂直滚动、折叠标题（部分）、面板
* 渲染：线条、圆、文本和凸多边形的抗锯齿渲染。
* 悬停提示
* 通过 [AccessKit](https://accesskit.dev/) 实现无障碍访问
* 标签文本选择
* 以及更多！


<img src="media/widget_gallery_0.23.gif" width="50%">

## 亮色主题

<img src="media/widget_gallery_0.23_light.png" width="50%">

## 依赖项
`egui` 有一组默认的最小依赖项：

- [`ab_glyph`](https://crates.io/crates/ab_glyph)
- [`ahash`](https://crates.io/crates/ahash)
- [`nohash-hasher`](https://crates.io/crates/nohash-hasher)
- [`parking_lot`](https://crates.io/crates/parking_lot)

较重的依赖项被保持在 `egui` 之外，即使是作为可选的部分。
`egui` 中没有任何不完全适用于 Wasm 的代码。

要将图像加载到 `egui` 中，可以使用官方的 [`egui_extras`](https://github.com/emilk/egui/tree/master/crates/egui_extras) 包。

另一方面，[`eframe`](https://github.com/emilk/egui/tree/master/crates/eframe) 有许多依赖项，包括 [`winit`](https://crates.io/crates/winit)，[`image`](https://crates.io/crates/image)，图形库，剪贴板库等等。

## egui 适合谁？

当您希望以简单的方式创建 GUI，或者您想将 GUI 添加到游戏引擎中时，egui 旨在成为最佳选择。

如果您不使用 Rust，则不适合使用 egui。如果您想要看起来本地的 GUI，egui 不适合您。如果您希望在升级时不会出现问题，egui 还不适合您（尚未）。

但是，如果您正在使用 Rust 编写交互式内容，需要简单的 GUI，则可能适合使用 egui。

## 集成

egui 旨在易于集成到您正在开发的任何现有游戏引擎或平台中。
egui 本身不知道或不关心它运行在哪个操作系统上，也不知道如何将事物渲染到屏幕上 - 这是 egui 集成的工作。

集成需要在每一帧执行以下操作：

- **输入**：收集输入（鼠标、触摸、键盘、屏幕大小等），并将其提供给 egui
- 调用应用程序 GUI 代码
- **输出**：处理 egui 输出（光标更改、粘贴、纹理分配等）
- **绘制**：渲染 egui 生成的三角形网格（请参阅 [OpenGL 示例](https://github.com/emilk/egui/blob/master/crates/egui_glow/src/painter.rs)）

### 官方集成

以下是官方的 egui 集成：

- [`eframe`](https://github.com/emilk/egui/tree/master/crates/eframe)：将相同的应用程序编译为 Web/Wasm 和桌面/本机。使用 `egui-winit` 和 `egui_glow` 或 `egui-wgpu`
- [`egui_glow`](https://github.com/emilk/egui/tree/master/crates/egui_glow)：在本地和 Web 上使用 [glow](https://github.com/grovesNL/glow) 渲染 egui，并制作本地应用程序
- [`egui-wgpu`](https://github.com/emilk/egui/tree/master/crates/egui-wgpu)：用于与 [wgpu](https://crates.io/crates/wgpu)（WebGPU API）集成
- [`egui-winit`](https://github.com/emilk/egui/tree/master/crates/egui-winit)：用于与 [winit](https://github.com/rust-windowing/winit) 集成

### 第三方集成


* [`amethyst_egui`](https://github.com/jgraef/amethyst_egui) for [the Amethyst game engine](https://amethyst.rs/)
* [`egui-ash`](https://github.com/MatchaChoco010/egui-ash) for [`ash`](https://github.com/ash-rs/ash) (a very lightweight wrapper around Vulkan)
* [`bevy_egui`](https://github.com/mvlabat/bevy_egui) for [the Bevy game engine](https://bevyengine.org/)
* [`egui_gl_glfw`](https://github.com/mrclean71774/egui_gl_glfw) for [GLFW](https://crates.io/crates/glfw)
* [`egui_glium`](https://github.com/fayalalebrun/egui_glium) for compiling native apps with [Glium](https://github.com/glium/glium)
* [`egui-glutin-gl`](https://github.com/h3r2tic/egui-glutin-gl/) for [glutin](https://crates.io/crates/glutin)
* [`egui_sdl2_gl`](https://crates.io/crates/egui_sdl2_gl) for [SDL2](https://crates.io/crates/sdl2)
* [`egui_sdl2_platform`](https://github.com/ComLarsic/egui_sdl2_platform) for [SDL2](https://crates.io/crates/sdl2)
* [`egui_vulkano`](https://github.com/derivator/egui_vulkano) for [Vulkano](https://github.com/vulkano-rs/vulkano)
* [`egui_winit_vulkano`](https://github.com/hakolao/egui_winit_vulkano) for [Vulkano](https://github.com/vulkano-rs/vulkano)
* [`egui-macroquad`](https://github.com/optozorax/egui-macroquad) for [macroquad](https://github.com/not-fl3/macroquad)
* [`egui-miniquad`](https://github.com/not-fl3/egui-miniquad) for [Miniquad](https://github.com/not-fl3/miniquad)
* [`egui_speedy2d`](https://github.com/heretik31/egui_speedy2d) for [Speedy2d](https://github.com/QuantumBadger/Speedy2D)
* [`egui-tetra`](https://crates.io/crates/egui-tetra) for [Tetra](https://crates.io/crates/tetra), a 2D game framework
* [`egui-winit-ash-integration`](https://github.com/MatchaChoco010/egui-winit-ash-integration) for [winit](https://github.com/rust-windowing/winit) and [ash](https://github.com/MaikKlein/ash)
* [`fltk-egui`](https://crates.io/crates/fltk-egui) for [fltk-rs](https://github.com/fltk-rs/fltk-rs)
* [`ggegui`](https://github.com/NemuiSen/ggegui) for the [ggez](https://ggez.rs/) game framework
* [`godot-egui`](https://github.com/setzer22/godot-egui) for [godot-rust](https://github.com/godot-rust/godot-rust)
* [`nannou_egui`](https://github.com/nannou-org/nannou/tree/master/nannou_egui) for [nannou](https://nannou.cc)
* [`notan_egui`](https://github.com/Nazariglez/notan/tree/main/crates/notan_egui) for [notan](https://github.com/Nazariglez/notan)
* [`screen-13-egui`](https://github.com/attackgoat/screen-13/tree/master/contrib/screen-13-egui) for [Screen 13](https://github.com/attackgoat/screen-13)
* [`egui_skia`](https://github.com/lucasmerlin/egui_skia) for [skia](https://github.com/rust-skia/rust-skia/tree/master/skia-safe)
* [`smithay-egui`](https://github.com/Smithay/smithay-egui) for [smithay](https://github.com/Smithay/smithay/)
* [`tauri-egui`](https://github.com/tauri-apps/tauri-egui) for [tauri](https://github.com/tauri-apps/tauri)
### 编写自己的 egui 集成

如果你正在开发的项目缺少一个 egui 集成，那么创建一个是很容易的！
请参阅 <https://docs.rs/egui/latest/egui/#integrating-with-egui>。

## 为什么选择即时模式

`egui` 是一个即时模式 GUI 库，与 *保留模式* GUI 库相对。保留模式和即时模式的区别最好通过按钮的例子来说明：在保留 GUI 中，你创建一个按钮，将其添加到某个 UI 中，并安装一些点击处理程序（回调）。按钮保留在 UI 中，要更改其上的文本，需要存储对它的某种引用。相比之下，在即时模式中，你立即显示按钮并与之交互，每帧都这样做（例如，每秒 60 次）。这意味着不需要任何点击处理程序，也不需要存储对其的任何引用。在 `egui` 中，这看起来像这样：`if ui.button("Save file").clicked() { save(file); }`。

关于即时模式的更详细描述可以在 [egui 文档](https://docs.rs/egui/latest/egui/#understanding-immediate-mode) 中找到。

这两种系统都有优点和缺点。

简单来说，即时模式 GUI 库易于使用，但功能较弱。

### 即时模式的优点

#### 可用性
即时模式的主要优点是应用程序代码变得极其简单：

* 你永远不需要任何点击处理程序和打断代码流的回调。
* 你不必担心悬挂的回调调用已经消失的内容。
* 你的 GUI 代码可以轻松地存在于一个简单的函数中（不需要一个专门用于 UI 的对象）。
* 你不必担心应用状态和 GUI 状态不同步（即 GUI 显示过时的东西），因为 GUI 不存储任何状态 - 它立即显示最新状态。

换句话说，大量的代码、复杂性和错误都消失了，你可以把时间专注于比编写 GUI 代码更有趣的事情上。

### 即时模式的缺点

#### 布局
即时模式的主要缺点是它使布局变得更加困难。假设你想在屏幕中央显示一个小对话框窗口。要正确定位窗口，GUI 库必须首先知道窗口的大小。为了知道窗口的大小，GUI 库必须首先布局窗口的内容。在保留模式中，这很容易：GUI 库进行窗口布局，定位窗口，然后检查交互（“OK 按钮被点击了吗？”）。

在即时模式中，你遇到了一个悖论：为了知道窗口的大小，我们必须做布局，但布局代码也检查交互（“OK 按钮被点击了吗？”），因此它需要在显示窗口内容之前就知道窗口位置。这意味着我们必须在知道大小之前决定在哪里显示窗口！

这是即时模式 GUI 的一个根本缺陷，任何试图解决它的尝试都会带来自己的缺点。

一个解决方法是存储大小并在下一帧使用它。这会产生正确布局的延迟一帧，导致第一次出现时会出现偶发性闪烁。对于某些事情（如窗口和网格布局）`egui` 会这样做。

你也可以两次调用布局代码（一次用于获取大小，一次用于进行交互），但这不仅更昂贵，而且实现起来也更复杂，而且在某些情况下两次可能还不够。`egui` 从不这样做。

对于“原子”部件（例如按钮），`egui` 在显示之前就知道大小，因此在 `egui` 中可以轻松地居中按钮、标签等，无需任何特殊的解决方法。

#### CPU 使用
由于即时模式 GUI 每帧都要进行全面的布局，所以布局代码必须快速。如果你的 GUI 很复杂，这可能会给 CPU 带来压力。特别是，在滚动区域中拥有非常庞大的 UI（非常长的回滚）可能会很慢，因为需要每帧布局内容。

如果你考虑到了这一点并且避免了巨大的滚动区域（或者只布局在视图中的部分），那么性能损失通常会很小。对于大多数情况，你可以期望 `egui` 占用每帧 1-2 毫秒，但 `egui` 仍然有很大的优化空间（这不是我目前专注的内容）。`egui` 仅在有交互（例如鼠标移动）或动画时重新绘制，因此如果你的应用程序处于空闲状态，则不会浪费 CPU。

如果你的 GUI 高度交互，那么即时模式实际上可能比保留模式更高效。打开任何网页并调整浏览器窗口大小，你会注意到浏览器在做布局时非常慢，耗费了大量 CPU。相比之下，在 `egui` 中调整窗口大小，你将以额外的 CPU 成本获得平滑的 60 FPS。

#### ID
即使在即时模式库（如 `egui`）中，也有一些 GUI 状态需要 GUI 库保留。这包括窗口的位置和大小以及用户在某些 UI 中滚动了多远。在这些情况下，你需要为 `egui` 提供一个唯一标识符的种子（在父 UI 中是唯一的）。例如：默认情况下，`egui` 使用窗口标题作为唯一标识符来存储窗口位置。如果你想要两个具有相同名称的窗口（或一个具有动态名称的窗口），你必须为 `egui` 提供一些其他的 ID 来源（一些唯一的整数或字符串）。

`egui` 还需要跟踪哪个部件正在与之交互（例如，哪个滑块正在被拖动）。`egui` 也为此使用唯一标识符，但在这种情况下，标识符是自动生成的，因此用户无需担心。特别是，拥有两个具有相同名称的按钮不是问题（这与 [`Dear ImGui`](https://github.com/ocornut/imgui) 相反）。

总的来说，ID 处理是一个罕见的不便，不是一个很大的缺点。
## 常见问题解答

还请参阅 [GitHub 讨论](https://github.com/emilk/egui/discussions/categories/q-a)。

### 我能否在 `egui` 中使用非拉丁字符？
可以！但你需要使用 [`Context::set_fonts`](https://docs.rs/egui/latest/egui/struct.Context.html#method.set_fonts) 安装自己的字体（`.ttf` 或 `.otf`）。

### 我能否自定义 egui 的外观？
可以！你可以使用 `Context::set_style` 自定义颜色、间距、字体和大小等一切。

这还不如 CSS 那么强大，[但这将会改进](https://github.com/emilk/egui/issues/3284)。

以下是一个示例（来自 https://github.com/a-liashenko/TinyPomodoro）：

<img src="media/pompodoro-skin.png" width="50%">

### 我如何在 `async` 中使用 egui？
如果在你的 GUI 代码中调用了 `.await`，UI 将会冻结，这对用户体验非常不好。相反，保持 GUI 线程非阻塞，并使用以下方式与任何并发任务（`async` 任务或其他线程）进行通信：
* 通道（例如 [`std::sync::mpsc::channel`](https://doc.rust-lang.org/std/sync/mpsc/fn.channel.html)）。确保使用 [`try_recv`](https://doc.rust-lang.org/std/sync/mpsc/struct.Receiver.html#method.try_recv) 以避免阻塞 GUI 线程！
* `Arc<Mutex<Value>>`（后台线程设置一个值；GUI 线程读取它）
* [`poll_promise::Promise`](https://docs.rs/poll-promise)
* [`eventuals::Eventual`](https://docs.rs/eventuals/latest/eventuals/struct.Eventual.html)
* [`tokio::sync::watch::channel`](https://docs.rs/tokio/latest/tokio/sync/watch/fn.channel.html)

### 我如何创建一个文件对话框？
[rfd](https://docs.rs/rfd/latest/rfd/) 的异步版本同时支持本机和 Wasm。请参见此处的示例应用 https://github.com/woelper/egui_pick_file，该示例还可通过 [gitub 页面](https://woelper.github.io/egui_pick_file/) 进行演示。

### 关于辅助功能，例如屏幕阅读器？
egui 包括对 [AccessKit](https://accesskit.dev/) 的可选支持，它目前在 Windows 和 macOS 上实现了原生辅助功能 API。这个功能在 eframe 中是默认启用的。对于 AccessKit 尚不支持的平台，包括 Web，在 [web demo](https://www.egui.rs/#demo) 中可以在“Backend”选项卡中启用试验性的内置屏幕阅读器。

关于 egui 辅助功能的最初讨论在 <https://github.com/emilk/egui/issues/167>。现在 AccessKit 支持已经合并，为未来的辅助功能工作提供了坚实的基础，请在特定辅助功能问题上提出新问题。

### [egui](https://docs.rs/egui) 和 [eframe](https://github.com/emilk/egui/tree/master/crates/eframe) 之间有什么区别？

`egui` 是一个用于布局和与按钮、滑块等进行交互的 2D 用户界面库。
`egui` 不知道自己是在 Web 还是在本地运行，也不知道如何收集输入或在屏幕上显示东西。
这是*集成*或*后端*的工作。

通常使用 `egui` 从游戏引擎中调用（例如 [`bevy_egui`](https://docs.rs/bevy_egui)），
但你也可以使用 `eframe` 独立使用 `egui`。`eframe` 具有 Web 和本地的集成，并处理输入和渲染。
`eframe` 中的 _frame_ 既表示你的 egui 应用程序所在的帧，也表示“框架”（`eframe` 是一个框架，`egui` 是一个库）。

### 如何在 egui 区域中渲染 3D 物体？
有多种方法可以将 egui 与 3D 结合使用。最简单的方法是使用一个 3D 库，并让 egui 放在 3D 视图的上方。例如，参见 [`bevy_egui`](https://github.com/mvlabat/bevy_egui) 或 [`three-d`](https://github.com/asny/three-d)。

如果你想将 3D 嵌入到 egui 视图中，有两种选择：

#### `Shape::Callback`
示例：
* <https://github.com/emilk/egui/blob/master/examples/custom_3d_glow/src/main.rs>

`Shape::Callback` 在 egui 绘制时会调用你的代码，使用背景渲染上下文来显示任何内容。当使用 [`eframe`](https://github.com/emilk/egui/tree/master/crates/eframe) 时，这将是 [`glow`](https://github.com/grovesNL/glow)。其他集成将提供其他渲染上下文，如果它们支持 `Shape::Callback` 的话。

#### 渲染到纹理
你也可以将 3D 场景渲染到纹理，并使用 [`ui.image(…)`](https://docs.rs/egui/latest/egui/struct.Ui.html#method.image) 显示它。你首先需要将本机纹理转换为 [`egui::TextureId`](https://docs.rs/egui/latest/egui/enum.TextureId.html)，如何做取决于你使用的集成。

示例：
* 使用 [`egui-miniquad`]( https://github.com/not-fl3/egui-miniquad)：https://github.com/not-fl3/egui-miniquad/blob/master/examples/render_to_egui_image.rs


## Other

### Conventions and design choices

All coordinates are in screen space coordinates, with (0, 0) in the top left corner

All coordinates are in logical "points" which may consist of many physical pixels.

All colors have premultiplied alpha, unless otherwise stated.

egui uses the builder pattern for construction widgets. For instance: `ui.add(Label::new("Hello").text_color(RED));` I am not a big fan of the builder pattern (it is quite verbose both in implementation and in use) but until Rust has named, default arguments it is the best we can do. To alleviate some of the verbosity there are common-case helper functions, like `ui.label("Hello");`.

Instead of using matching `begin/end` style function calls (which can be error prone) egui prefers to use `FnOnce` closures passed to a wrapping function. Lambdas are a bit ugly though, so I'd like to find a nicer solution to this. More discussion of this at <https://github.com/emilk/egui/issues/1004#issuecomment-1001650754>.

egui uses a single `RwLock` for short-time locks on each access of `Context` data. This is to leave implementation simple and transactional and allow users to run their UI logic in parallel. Instead of creating mutex guards, egui uses closures passed to a wrapping function, e.g. `ctx.input(|i| i.key_down(Key::A))`. This is to make it less likely that a user would accidentally double-lock the `Context`, which would lead to a deadlock.

### Inspiration

The one and only [Dear ImGui](https://github.com/ocornut/imgui) is a great Immediate Mode GUI for C++ which works with many backends. That library revolutionized how I think about GUI code and turned GUI programming from something I hated to do to something I now enjoy.

### Name

The name of the library and the project is "egui" and pronounced as "e-gooey". Please don't write it as "EGUI".

The library was originally called "Emigui", but was renamed to "egui" in 2020.

## Credits

egui author and maintainer: Emil Ernerfeldt ([@emilk](https://github.com/emilk)).

Notable contributions by:

* [@n2](https://github.com/n2): [Mobile web input and IME support](https://github.com/emilk/egui/pull/253)
* [@optozorax](https://github.com/optozorax): [Arbitrary widget data storage](https://github.com/emilk/egui/pull/257)
* [@quadruple-output](https://github.com/quadruple-output): [Multitouch](https://github.com/emilk/egui/pull/306)
* [@EmbersArc](https://github.com/EmbersArc): [Plots](https://github.com/emilk/egui/pulls?q=+is%3Apr+author%3AEmbersArc)
* [@AsmPrgmC3](https://github.com/AsmPrgmC3): [Proper sRGBA blending for web](https://github.com/emilk/egui/pull/650)
* [@AlexApps99](https://github.com/AlexApps99): [`egui_glow`](https://github.com/emilk/egui/pull/685)
* [@mankinskin](https://github.com/mankinskin): [Context menus](https://github.com/emilk/egui/pull/543)
* [@t18b219k](https://github.com/t18b219k): [Port glow painter to web](https://github.com/emilk/egui/pull/868)
* [@danielkeller](https://github.com/danielkeller): [`Context` refactor](https://github.com/emilk/egui/pull/1050)
* [@MaximOsipenko](https://github.com/MaximOsipenko): [`Context` lock refactor](https://github.com/emilk/egui/pull/2625)
* [@mwcampbell](https://github.com/mwcampbell): [AccessKit](https://github.com/AccessKit/accesskit) [integration](https://github.com/emilk/egui/pull/2294)
* [@hasenbanck](https://github.com/hasenbanck), [@s-nie](https://github.com/s-nie), [@Wumpf](https://github.com/Wumpf): [`egui-wgpu`](https://github.com/emilk/egui/tree/master/crates/egui-wgpu)
* [@jprochazk](https://github.com/jprochazk): [egui image API](https://github.com/emilk/egui/issues/3291)
* And [many more](https://github.com/emilk/egui/graphs/contributors?type=a).

egui is licensed under [MIT](LICENSE-MIT) OR [Apache-2.0](LICENSE-APACHE).

* The flattening algorithm for the cubic bezier curve and quadratic bezier curve is from [lyon_geom](https://docs.rs/lyon_geom/latest/lyon_geom/)

Default fonts:

* `emoji-icon-font.ttf`: [Copyright (c) 2014 John Slegers](https://github.com/jslegers/emoji-icon-font) , MIT License
* `Hack-Regular.ttf`: <https://github.com/source-foundry/Hack>, [MIT Licence](https://github.com/source-foundry/Hack/blob/master/LICENSE.md)
* `NotoEmoji-Regular.ttf`: [google.com/get/noto](https://google.com/get/noto), [SIL Open Font License](https://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=OFL)
* `Ubuntu-Light.ttf` by [Dalton Maag](http://www.daltonmaag.com/): [Ubuntu font licence](https://ubuntu.com/legal/font-licence)

---

<div align="center">
<a href="https://www.rerun.io/"><img src="media/rerun_io_logo.png" width="440"></a>

egui development is sponsored by [Rerun](https://www.rerun.io/), a startup building<br>
an SDK for visualizing streams of multimodal data.
</div>## 其他

### 规范和设计选择

所有坐标都以屏幕空间坐标表示，其中 (0, 0) 位于左上角。

所有颜色都具有预乘 alpha，除非另有说明。

egui 使用生成器模式来构建小部件。例如：`ui.add(Label::new("Hello").text_color(RED));` 我不是很喜欢生成器模式（它在实现和使用上都相当冗长），但在 Rust 拥有命名和默认参数之前，这是我们能做的最好的。为了减少一些冗长，有一些常见情况下的辅助函数，比如 `ui.label("Hello");`。

而不是使用匹配的 `begin/end` 风格的函数调用（这可能会出错），egui 更喜欢使用传递给包装函数的 `FnOnce` 闭包。虽然 lambda 有点丑陋，但我希望能找到一个更好看的解决方案。有关此的更多讨论，请参见 <https://github.com/emilk/egui/issues/1004#issuecomment-1001650754>。

egui 使用一个单一的 `RwLock` 对 `Context` 数据的每次访问进行短时间锁定。这是为了保持实现简单和事务性，并允许用户并行运行他们的 UI 逻辑。而不是创建互斥锁守卫，egui 使用传递给包装函数的闭包，例如 `ctx.input(|i| i.key_down(Key::A))`。这是为了减少用户意外双重锁定 `Context` 的可能性，这将导致死锁。

### 灵感

唯一的 [Dear ImGui](https://github.com/ocornut/imgui) 是一个出色的 C++ 即时模式 GUI，适用于许多后端。该库改变了我对 GUI 代码的看法，将 GUI 编程从我讨厌做的事情转变为我现在喜欢做的事情。

### 名称

该库和项目的名称是 "egui"，发音为 "e-gooey"。请不要将其写成 "EGUI"。

该库最初被称为 "Emigui"，但在 2020 年更名为 "egui"。

## 鸣谢

egui 作者和维护者：Emil Ernerfeldt ([@emilk](https://github.com/emilk))。

值得注意的贡献者包括：

* [@n2](https://github.com/n2)：[移动 Web 输入和 IME 支持](https://github.com/emilk/egui/pull/253)
* [@optozorax](https://github.com/optozorax)：[任意小部件数据存储](https://github.com/emilk/egui/pull/257)
* [@quadruple-output](https://github.com/quadruple-output)：[多点触控](https://github.com/emilk/egui/pull/306)
* [@EmbersArc](https://github.com/EmbersArc)：[绘图](https://github.com/emilk/egui/pulls?q=+is%3Apr+author%3AEmbersArc)
* [@AsmPrgmC3](https://github.com/AsmPrgmC3)：[Web 的正确 sRGBA 混合](https://github.com/emilk/egui/pull/650)
* [@AlexApps99](https://github.com/AlexApps99)：[`egui_glow`](https://github.com/emilk/egui/pull/685)
* [@mankinskin](https://github.com/mankinskin)：[上下文菜单](https://github.com/emilk/egui/pull/543)
* [@t18b219k](https://github.com/t18b219k)：[将 glow 渲染器移植到 Web](https://github.com/emilk/egui/pull/868)
* [@danielkeller](https://github.com/danielkeller)：[`Context` 重构](https://github.com/emilk/egui/pull/1050)
* [@MaximOsipenko](https://github.com/MaximOsipenko)：[`Context` 锁重构](https://github.com/emilk/egui/pull/2625)
* [@mwcampbell](https://github.com/mwcampbell)：[AccessKit](https://github.com/AccessKit/accesskit) [集成](https://github.com/emilk/egui/pull/2294)
* [@hasenbanck](https://github.com/hasenbanck)、[@s-nie](https://github.com/s-nie)、[@Wumpf](https://github.com/Wumpf)：
[`egui-wgpu`](https://github.com/emilk/egui/tree/master/crates/egui-wgpu)
* [@jprochazk](https://github.com/jprochazk)：[egui 图像 API](https://github.com/emilk/egui/issues/3291)
* 以及[其他许多人](https://github.com/emilk/egui/graphs/contributors?type=a)。

egui 使用 [MIT](LICENSE-MIT) 或 [Apache-2.0](LICENSE-APACHE) 许可证。

* 来自 [lyon_geom](https://docs.rs/lyon_geom/latest/lyon_geom/) 的立方贝塞尔曲线和二次贝塞尔曲线的展平算法。

默认字体：

* `emoji-icon-font.ttf`：[版权所有 (c) 2014 John Slegers](https://github.com/jslegers/emoji-icon-font)，MIT 许可证
* `Hack-Regular.ttf`：来自 <https://github.com/source-foundry/Hack>，[MIT 许可证](https://github.com/source-foundry/Hack/blob/master/LICENSE.md)
* `NotoEmoji-Regular.ttf`：[google.com/get/noto](https://google.com/get/noto)，[SIL 开源字体许可证](https://scripts.sil.org/cms/scripts/page.php?site_id=nrsi&id=OFL)
* `Ubuntu-Light.ttf` 由 [Dalton Maag](http://www.daltonmaag.com/) 创建：[Ubuntu 字体许可证](https://ubuntu.com/legal/font-licence)

---

<div align="center">
<a href="https://www.rerun.io/"><img src="media/rerun_io_logo.png" width="440"></a>

egui 开发由 [Rerun](https://www.rerun.io/) 赞助，一个正在构建<br>
用于可视化多模态数据流的 SDK 的创业公司。
</div>

