# Nature Webview Binding

[!\[Version\](https://img.shields.io/badge/version-0.12.0-blue.svg)](https://github.com/your-repo)
[!\[License\](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)

为 [Nature](https://github.com/nature-lang) 语言提供的 Webview 绑定库，基于 [webview](https://github.com/webview/webview) C 库实现。该库允许你在 Nature 语言中轻松创建跨平台的桌面应用程序，使用 Web 技术构建用户界面。

## 特性

- 🚀 **跨平台支持**：支持 macOS、Linux&#x20;
- 🌐 **现代 Web 技术**：使用 HTML、CSS 和 JavaScript 构建原生窗口
- ⚡ **轻量级**：无需外部依赖，直接使用系统原生 WebView
- 🔗 **双向绑定**：支持从 JavaScript 调用 Native 函数
- 📦 **简单易用**：简洁的 API 设计，快速上手

## 安装

### 前置要求

- [Nature 编译器](https://github.com/nature-lang)
- C 编译器（GCC、Clang 或 MSVC）

### 获取代码

```bash
git clone https://github.com/wzzc-dev/nature-webview.git
cd nature-webview
```

### 构建静态库

项目预编译了各平台的静态库，位于 `lib/` 目录下。如需自行构建：

```bash
cd webview
mkdir build && cd build
cmake ..
cmake --build .
cp libwebview.a ../../lib/$(uname -s)_$(uname -m)/
```

## 快速开始

### 基础示例

创建一个简单的 WebView 窗口：

```javascript
import 'webview.n'
import fmt

fn main() {
    fmt.printf("Hello, Nature Webview!\n")
    
    // 创建 webview 实例（debug 模式关闭）
    webview.webview_t window = webview.create(0, null)
    
    // 设置窗口标题
    webview.set_title(window, "Hello World".to_cstr())
    
    // 设置 HTML 内容
    webview.set_html(window, "<html><body><h1>Hello, Nature!</h1></body></html>".to_cstr())
    
    // 运行消息循环
    webview.run(window)
    
    // 清理资源
    webview.destroy(window)
}
```

### 编译运行

#### macOS (Apple Silicon)

```bash
nature build --ldflags '-framework WebKit -framework Cocoa -lc++' main.n
./main
```

#### macOS (Intel)

```bash
nature build --ldflags '-framework WebKit -framework Cocoa -lc++' -arch x86_64 main.n
./main
```

#### Linux

```bash
nature build --ldflags '-lwebkit2gtk-4.1' main.n
./main
```

## API 文档

### 核心函数

#### `create(debug, window)`

创建一个新的 webview 实例。

**参数：**

- `debug: i32` - 是否启用开发者工具（0 或 1）
- `window: webview_t` - 父窗口指针（通常为 null）

**返回：** `webview_t` - webview 实例指针

---

#### `set_title(w, title)`

设置窗口标题。

**参数：**

- `w: webview_t` - webview 实例
- `title: libc.cstr` - 窗口标题

---

#### `set_size(w, width, height, hint)`

设置窗口大小。

**参数：**

- `w: webview_t` - webview 实例
- `width: i32` - 窗口宽度
- `height: i32` - 窗口高度
- `hint: i32` - 尺寸提示（WEBVIEW\_HINT\_NONE, WEBVIEW\_HINT\_MIN, WEBVIEW\_HINT\_MAX）

---

#### `set_html(w, html)`

设置 HTML 内容。

**参数：**

- `w: webview_t` - webview 实例
- `html: libc.cstr` - HTML 字符串

---

#### `navigate(w, url)`

导航到指定 URL。

**参数：**

- `w: webview_t` - webview 实例
- `url: libc.cstr` - 目标 URL

---

#### `run(w)`

运行主事件循环。

**参数：**

- `w: webview_t` - webview 实例

---

#### `destroy(w)`

销毁 webview 实例并释放资源。

**参数：**

- `w: webview_t` - webview 实例

---

### 高级功能

#### JavaScript 执行

```javascript
// 初始化时执行 JS
webview.init(window, "console.log('Initialized');".to_cstr())

// 运行时执行 JS
webview.eval(window, "document.body.style.background = '#f0f0f0';".to_cstr())
```

## 项目结构

```javascript
nature-webview/
├── lib/                  # 预编译静态库
│   ├── darwin_arm64/    # macOS ARM64
│   ├── darwin_amd64/    # macOS Intel
│   ├── linux_x86_64/    # Linux x86_64
├── webview/            # 原始 webview C 库
│   ├── core/           # 核心实现
│   ├── examples/       # C/C++ 示例
│   └── test_driver/    # 测试
├── main.n             # 示例入口
├── webview.n          # Nature 绑定定义
├── package.toml       # 项目配置
└── README.md          # 本文件
```

## 依赖

### macOS

- WebKit.framework
- Cocoa.framework

### Linux

- libwebkit2gtk-4.1

## 故障排除

### macOS 编译错误

确保已安装 Xcode 命令行工具：

```bash
xcode-select --install
```

### Linux 缺少 webkit2gtk

```bash
# Ubuntu/Debian
sudo apt-get install libwebkit2gtk-4.1-dev

# Fedora
sudo dnf install webkit2gtk4.1-devel

# Arch Linux
sudo pacman -S webkit2gtk-4.1
```

### 贡献

欢迎贡献！请阅读 [CONTRIBUTING.md](webview/CONTRIBUTING.md) 了解详情。

1. Fork 本仓库
2. 创建特性分支 (`git checkout -b feature/AmazingFeature`)
3. 提交更改 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 提交 Pull Request

## 许可证

本项目基于 [MIT 许可证](LICENSE) 开源。

## 致谢

- [webview/webview](https://github.com/webview/webview) - 提供底层 C 库
- [Nature](https://github.com/nature-lang) - 编程语言
- 所有贡献者

## 联系方式

- 作者：wzzc-dev
- 问题反馈：[GitHub Issues](https://github.com/your-repo/issues)

---

**注意**：本项目处于开发阶段，API 可能会发生变化。建议在生产环境使用前进行充分测试。
