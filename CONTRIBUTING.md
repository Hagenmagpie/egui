# 贡献指南

## 介绍

`egui` 自2018年末以来一直是我周末的一项断断续续的项目。我对任何帮助都心存感激，但请记住，有时我会因为其他事情而反应较慢！

/ Emil

## 如何为 egui 做出贡献

你想为 egui 做出贡献，但不知道如何开始？首先：谢谢你！我专门为此创建了一个特殊的问题：[链接](https://github.com/emilk/egui/issues/3742)。但请确保你先阅读本文件 :)

## 讨论

你可以在 [GitHub Discussions](https://github.com/emilk/egui/discussions) 上提问、分享截图等等。

`egui` 的 Discord 频道在 [这里](https://discord.gg/vbuv9Xan65)。

## 提交问题

[问题](https://github.com/emilk/egui/issues) 用于报告错误和提出功能请求。问题不用于提问（使用 [Discussions](https://github.com/emilk/egui/discussions) 或 [Discord](https://discord.gg/vbuv9Xan65)）。

请务必确保所创建的问题与已有的问题不相似。

如果你要报告错误，请提供重现错误的方法。

## 提交 Pull Request

对于小事情，可以直接打开一个 PR。对于更大的事情，请先提出一个问题（或找到一个已存在的问题），并宣布你计划着手解决某事。这样我们就可以避免多人做重复工作，并且你在开始工作之前可能会得到有用的反馈。

浏览 [`ARCHITECTURE.md`](ARCHITECTURE.md) 以了解所有部分如何连接起来。

你可以通过运行 `./scripts/check.sh` 在本地测试你的代码。

当你有一个可行的东西时，打开一个草稿 PR。你可能会在早期获得一些有用的反馈！当你觉得 PR 准备就绪时，进行自审代码，然后将其开放供审阅。

不要担心 PR 中有很多小的提交 - 它们在合并后将被压缩成一个提交。

请保持 PR 的大小和焦点。它越小，越有可能被合并。

## PR 审查

大多数 PR 审查都由我来完成，但我非常感谢任何帮助我审查 PR！

向项目添加复杂性很容易，但请记住，每添加一行代码都需要永久维护，所以我们对合并的代码有很高的要求！

审查时，我们关注以下几点：
* PR 标题和描述应该有帮助
* 在 PR 描述中记录了重大更改
* 代码应易读
* 代码应有有用的文档字符串
* 代码应遵循 [代码风格](CONTRIBUTING.md#code-style)

请注意，每个新的 egui 版本都会有一些重大更改，因此我们不介意在 PR 中有一些这样的更改。当然，如果可能的话，我们仍然会尽量避免它们，如果无法避免，我们会首先使用 `#[deprecated]` 属性弃用旧代码。

## 创建 egui 的集成

请参阅 [这里](https://docs.rs/egui/latest/egui/#integrating-with-egui) 了解如何编写自己的 egui 集成。

如果你为某个引擎或渲染器创建了一个 `egui` 集成，请与世界分享！通过向 [`README.md`](README.md#integrations) 提交 PR 将其添加为一个链接，这样其他人就可以轻松找到它。

## 测试 web 视图器

* 使用 `scripts/build_demo_web.sh` 构建
* 使用 `scripts/start_server.sh` 进行主机托管
* 打开 <http://localhost:8888/index.html>

## 代码风格

虽然使用即时模式 GUI 是简单的，但实现一个却更加棘手。你需要考虑很多微妙的边缘情况。`egui` 源代码有点凌乱，部分原因是它仍在不断发展。

* 在编写自己的代码之前阅读一些代码
* 将代码保持干净
* 编写惯用的 rust
* 遵循 [Rust API Guidelines](https://rust-lang.github.io/api-guidelines/)
* 在所有 `fn`、`struct`、`enum` 等周围添加空行
* 使用 `// 像这样注释。` 而不是 `//像这样`
* 使用 `TODO` 而不是 `FIXME`
* 将你的 github handle 添加到你写的 `TODO` 中，例如：`TODO(emilk): clean this up`
* 避免使用 `unsafe`
* 避免使用 `unwrap` 和任何可能导致 panic 的代码
* 为所有东西使用良好的命名
* 为类型、`struct` 字段和所有 `pub fn` 添加文档字符串
* 添加一些示例代码（文档测试）
* 在使函数变得更长之前，请考虑添加一个辅助函数
* 如果你只在一个函数中使用它，请将 `use` 语句放在该函数中。这样可以提高局部性，使代码更容易阅读和移动
* 当导入一个 `trait` 来使用它的 trait 方法时，请这样做：`use Trait as _;`。这样让读者知道你为什么导入它，即使它看起来没有被使用
* 避免双重否定
* 将 `if !condition {} else {}` 改为 `if condition {} else {}`
* 一组东西应按词典顺序排序（例如 `Cargo.toml` 中的 crate 依赖项）
* 在适当时打破以上规则

### 好的写法：

```rust
/// 事物的名称。
pub fn name(&self) -> &str {
    &self.name
}

fn foo(&self) {
    // TODO(emilk): this can be optimized
}
```

### Bad:
``` rust
//gets the name
pub fn get_name(&self) -> &str {
    &self.name
}
fn foo(&self) {
    //FIXME: this can be optimized
}
```

### Coordinate system
屏幕左上角的坐标是 `(0.0, 0.0)`，`Vec2::X` 增加到右边，`Vec2::Y` 增加到下面。

`egui` 使用逻辑 _点_ 作为其坐标系统。
这些点通过 `pixels_per_point` 比例因子与物理 _像素_ 相关联。
例如，高 DPI 屏幕可以有 `pixels_per_point = 2.0`，这意味着每个逻辑点有两个物理屏幕像素。

角度以弧度表示，从 X 轴顺时针测量，角度为 0。


### 避免使用 `unwrap`、`expect` 等
代码不应该导致 panic 或崩溃，这意味着任何 `unwrap` 或 `expect` 的实例都是潜在的定时炸弹。即使你结构化了代码使它们不可能发生，任何读者仍然必须非常仔细地阅读代码来证明 `unwrap` 不会引发 panic。通常，你可以重写你的代码以避免它。对于切片的索引（超出边界时会引发 panic），通常最好使用 `.get()`。

例如：

``` rust
let first = if vec.is_empty() {
    return;
} else {
    vec[0]
};
```

可以更好地写成：

``` rust
let Some(first) = vec.first() else {
    return;
};
```
