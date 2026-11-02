<div align="center">
  <img src="assets/logo/image13.png" width="108" height="108" alt="NexAir Icon" style="border-radius: 24px; box-shadow: 0 18px 40px -10px rgba(15,111,212,0.45);" />
  <h1 style="margin-top: 16px; font-size: 34px; font-weight: 800; letter-spacing: -0.02em;">NexAir</h1>
  <p style="font-size: 16px; font-weight: 500; color: #6e6e73;">
    <strong>专为 macOS 打造的高保真多沙盒指纹浏览器与 AI Agent 原生运行基座</strong>
  </p>
  <p style="max-width: 680px; font-size: 13.5px; line-height: 1.6; color: #86868b;">
    macOS 14+ Sonoma 与 macOS 15+ Sequoia 原生架构 · Apple WebKit 真实内核 · 绝对零 JS 注入 · 全协议代理 Fail-Closed 严格熔断 · macOS 26+ Liquid Glass 流体玻璃设计
  </p>
  <p>
    <a href="https://github.com/XideaDev/NexAir.APP/releases/latest"><img src="https://img.shields.io/badge/Release-v1.3.6-0F6FD4?style=for-the-badge&logo=apple" alt="Release v1.3.6" /></a>
    <img src="https://img.shields.io/badge/macOS-14.0%2B-1d1d1f?style=for-the-badge&logo=apple" alt="macOS 14.0+" />
    <img src="https://img.shields.io/badge/Architecture-Universal%20(arm64%20%2B%20x86__64)-34C759?style=for-the-badge" alt="Universal" />
    <img src="https://img.shields.io/badge/Engine-Native%20WebKit-007AFF?style=for-the-badge&logo=safari" alt="Native WebKit" />
    <img src="https://img.shields.io/badge/Invariants-Zero%20JS%20Injection-FF9500?style=for-the-badge" alt="Zero JS Injection" />
    <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-gray?style=for-the-badge" alt="License MIT" /></a>
  </p>
  <p>
    <a href="README.md">English</a> · <strong>简体中文</strong> · <a href="#-快速下载与安装">直接下载</a> · <a href="#-系统核心架构">系统架构</a> · <a href="#-ai-agent-原生支持与-mcp-生态">AI Agent & MCP</a>
  </p>
</div>

---

## ⚡ 核心分水岭：为什么市面改装方案极易被风控检测？

市面上 99% 的多账号/指纹浏览器（如 AdsPower、比特、Hubstudio 等）均基于 Chromium 内核改造。为了篡改硬件特征，它们在页面加载前通过 `Page.addScriptToEvaluateOnNewDocument` 向网页全局执行上下文强行注入 JS 脚本。

现代企业级风控系统（**Cloudflare Turnstile、DataDome、Kasada、Akamai**）极易通过原型链交叉比对（`Function.prototype.toString`、`Object.getOwnPropertyDescriptor`、`iframe` 跨域原型检测）捕捉到“浏览器已被脚本篡改”。

```
市面改装方案 (Chromium) ──► 页面加载前 JS 篡改原型链 ──► 触发 Cloudflare / DataDome 熔断拦截
NexAir 原生 WebKit 方案 ──► 系统级 SPI 与环境变量硬化 ──► 真实 Safari 流量，天然混入苹果亿级生态池
```

### NexAir 三大不可逾越的架构铁律：

1. 🛡️ **绝对零 JS 注入（Zero JS Injection Law）**：
   **严禁**向常规浏览页面注入任何 `WKUserScript`，严禁在页面全局环境中重写 `window`、`navigator`、`screen`、`WebGL` 原型链！所有环境特征（语言、时区、屏幕几何尺寸）均通过 macOS 系统级环境变量（`-AppleLanguages`、`TZ`）与 WebKit 运行时底层私有 SPI 动态安全桥接生效。目标网站看到的，是一台真实无异的 Mac 原生 Safari 浏览器。
2. 🔒 **网络严格 Fail-Closed（断网绝对阻断）**：
   每个沙盒环境独立分配专属 `sing-box` 进程或独立 SOCKS5 代理。代理连接中断或解析失败时，网页立即终止加载并报错，**绝对禁止自动降级或回退到本机直连网络**，彻底杜绝真实物理 IP 泄露；增加 53 端口 UDP/TCP 强制劫持与 `block` 兜底路由，严防 DNS 旁路逃逸。
3. 🔐 **沙盒保险箱与零明文落盘（Private Vault Storage）**：
   代理与节点凭据只落盘于 POSIX `0600` 权限独立沙盒加密保险箱（`credentials.vault`）与 macOS 系统钥匙串。传递给子进程时仅通过标准输入（stdin 管道，配置 `F_SETNOSIGPIPE`）。明文密码与密钥绝不记录于日志或命令行参数中。

---

## 📸 真实产品界面画廊 (v1.3.6 最新实拍)

<div align="center">
  <img src="assets/screenshots/dashboard-1.3.6.png" width="94%" alt="NexAir v1.3.6 真实仪表盘" style="border-radius: 14px; box-shadow: 0 20px 50px rgba(0,0,0,0.25);" />
  <p><em>NexAir v1.3.6 现代化控制台 · 纯色彩状态灯表达、零文字杂音、Liquid Glass 流体拓扑</em></p>
</div>

<br />

<div align="center">
  <table width="100%" style="border: none;">
    <tr>
      <td width="50%" align="center" style="border: none;">
        <img src="assets/screenshots/browser-multi-1.3.6.png" width="100%" alt="1:1 Safari 胶囊标签栏" style="border-radius: 10px;" />
        <br /><strong>1:1 Safari Chrome 胶囊标签栏 (多标签平铺自适应)</strong>
      </td>
      <td width="50%" align="center" style="border: none;">
        <img src="assets/screenshots/browser-single-1.3.6.png" width="100%" alt="Safari 原生单标签页模式" style="border-radius: 10px;" />
        <br /><strong>单标签自动紧凑收拢模式与纯矢量国旗胶囊</strong>
      </td>
    </tr>
  </table>
</div>

---

## 🖥️ 系统核心架构 (System Architecture)

NexAir 采用健壮的 **单二进制、多进程协同（Single Binary, Multi-Process）** 工业级架构：

```
                      ┌──────────────────────────────────────────────┐
                      │          NexAir App (单一可执行文件)           │
                      └───────┬──────────────────────────────┬───────┘
                              │                              │
         [默认无参启动]        │                              │ [run --profile <ID>]
                              ▼                              ▼
             ┌────────────────────────────────┐  ┌────────────────────────────────┐
             │       NexAir ManagerGUI        │  │         NexAir Runner          │
             │   (SwiftUI 现代化管理控制台)    │  │   (独立 AppKit 进程，每个沙盒独立)│
             └────────────────────────────────┘  └───────┬────────────────┬───────┘
                                                         │                │
                                                         ▼                ▼
                                            ┌──────────────────┐  ┌──────────────┐
                                            │   NexAirWebKit   │  │sing-box 进程 │
                                            │ (WKWebView 编排) │  │(专属本地端口)│
                                            └────────┬─────────┘  └──────────────┘
                                                     ▼
                                            ┌──────────────────┐
                                            │NexAirObjCSupport │
                                            │(私有 SPI 动态桥接)│
                                            └────────┬─────────┘
                                                     ▼
                                            ┌──────────────────┐
                                            │   NexAirCore     │
                                            │ (沙盒模型/钥匙串) │
                                            └──────────────────┘
```

### 模块职责界定与依赖边界

| 模块名称 | 源码目录 | 核心职责与技术边界 |
|---|---|---|
| **`NexAirCore`** | `Sources/NexAirCore` | **数据模型与沙盒凭据**：纯依赖 Foundation 与 Security。定义 `Profile`、`ProfileGroup`、34 款 Mac 设备机型库、`BrowsingHistoryStore` 历史/会话、`ProfileLock` 排他锁及 `MCPServerRegistry`。 |
| **`NexAirNetwork`** | `Sources/NexAirNetwork` | **网络与代理生命周期**：负责节点解析（SS, VLESS, VMess, Trojan, SOCKS）、`sing-box` 动态配置生成（防 DNS 泄露）、专属子进程管理、端口调度与 GeoIP 定位。 |
| **`NexAirObjCSupport`** | `Sources/NexAirObjCSupport` | **原生引擎运行时桥接**：纯 Objective-C 底层实现。负责 XOR 混淆符号解码、dlsym 动态解析、签名校验、Method Swizzling 动态替换 `NSScreen` 尺寸/色域、C 导出符号配置系统字体白名单。 |
| **`NexAirWebKit`** | `Sources/NexAirWebKit` | **WebKit 编排与行为装配**：组装 `WKWebViewConfiguration`，绑定物理隔离 `WKWebsiteDataStore`、网络代理配置、`SPIPrivacyAdapter` 隐私能力适配与屏幕几何模拟。 |
| **`NexAir`** (App) | `Sources/NexAir` | **可执行应用总装**：SwiftUI 管理控制台（`ManagerGUI/`）、AppKit 原生 Safari 式多窗口/多标签页（`Browser/`）、Runner 进程启动入口与面向 AI Agent 的 IPC 服务。 |

---

## ✨ 核心能力与功能特性

### 1. 34 款真实 Mac 机型硬件层拟态
完美模拟真实 Apple Silicon 与 Intel 架构 Mac 设备，视口几何比例、Retina 像素比、Display P3 广色域及 HDR/EDR 高动态范围与真实真机无异：
- **MacBook Pro 16" & 14"**（Apple Silicon M 系列芯片、Liquid Retina XDR 极亮屏、Display P3、刘海屏几何特征）
- **MacBook Air 15" & 13"**（超薄便携、Display P3、SDR 屏幕）
- **iMac 24" 4.5K Retina**（4480×2520 @2x 桌面超高清屏幕）
- **Mac Studio（搭配 5K Studio Display）**（5120×2880 @2x）
- **Mac Pro（搭配 6K Pro Display XDR）**（6016×3384 @2x，P3 HDR）

### 2. 语言与时区自动跟随出口 IP
只需选择代理节点（美国、日本、英国、新加坡、香港、台湾、德国等），NexAir 启动时自动通过加密通道探测出口 IP 地理位置，毫秒级同步匹配对应的 **浏览器语言栈**（`Accept-Language`）与 **系统底层时区**（`TZ`）。

### 3. 系统字体白名单隔离（Anti-Font-Enumeration）
系统层自动屏蔽用户电脑上自行安装的所有第三方字体，保持纯净 macOS 系统自带字体栈（涵盖完整中日韩基础字库），阻断网站通过 Canvas/CSS 字体探测枚举来标识设备指纹。

### 4. 内置全协议 Air Engine
无需安装开启 Clash、Surge、v2ray 等任何外部客户端。支持直接导入通用订阅链接，或直接粘贴单条/批量节点链接：
- `VLESS`（Reality / uTLS / Vision / gRPC / WebSocket）
- `VMess`（WebSocket / gRPC / TLS）
- `Trojan`
- `Shadowsocks` & `SS2022`（2022-blake3-aes-128-gcm / 2022-blake3-aes-256-gcm）
- `AnyTLS`、`SOCKS5`、`HTTP`

### 5. 1:1 Safari 原生极客顶栏与防崩标签栏
- **Safari 胶囊标签栏**：高 30pt、10.5pt 连续超椭圆圆角（Continuous Squircle）、`maxWidth: .infinity` 轨道自适应平铺，智能相邻避让分割线。
- **26pt 矢量国旗胶囊**：内嵌纯自绘 17×12.8pt 真实国旗图标，支持点击展开实时出站详情卡片。
- **零轮询多标签性能防崩**：彻底摒弃定时器，增量 Diff 驱动，15~20 个标签高负载多开 CPU 占用近乎于 0。

---

## 🤖 AI Agent 原生支持与 MCP 生态

NexAir 天然是面向自主 AI Agent（Claude Code、OpenAI Operator、Codex、Cursor、OpenCode、PI-Desktop）的物理级浏览器基座。

### 全生态 MCP 服务器自动注册 (Zero-Config)
应用启动时，`MCPServerRegistry` 会自动将 **`nexair`** 注册到系统各大智能体与 IDE 环境中，无需手动复制粘贴配置：
- **标准全局 Agent 规范**：写入 `~/.agents/servers/nexair.json`
- **项目级 Agent 规范**：写入 `{Workspace}/.agents/servers/nexair.json`
- **Codex 自动注入**：自动追加至 `~/.codex/config.toml`
- **OpenCode 自动注入**：自动合并至 `~/.config/opencode/opencode.json`
- **Claude Desktop & Cursor**：自动注册至 `claude_desktop_config.json` 与 `~/.cursor/mcp.json`
- **CLI 全局软链接**：自动建立 `~/.local/bin/nexair`

```bash
# 验证 MCP Stdio 握手响应
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}' | /Applications/NexAir.app/Contents/MacOS/NexAir mcp
```

### Agent CLI 自动化指令速查

```bash
NX="/Applications/NexAir.app/Contents/MacOS/NexAir"

# 1. 查询当前已配置的沙盒环境
"$NX" agent spaces

# 2. 在后台静默打开指定沙盒与网页
"$NX" agent open "Workspace US" https://example.com --background

# 3. 获取无障碍语义快照 (标签标注为 e1, e2...)
"$NX" agent snapshot "Workspace US"

# 4. 驱动表单输入与元素点击
"$NX" agent fill "Workspace US" e1 "user@example.com"
"$NX" agent click "Workspace US" e2

# 5. 原生物理坐标点击 (突破 Canvas 复杂人机验证)
"$NX" agent click-coord "Workspace US" 420 310

# 6. 截取视口高保真快照
"$NX" agent screenshot "Workspace US" /tmp/view.png
```

---

## 📥 快速下载与安装

直接下载打包好的 Universal 独立应用程序（同时支持 **Apple 芯片 M 系列** 与 **Intel 芯片 Mac**，要求 **macOS 14 Sonoma** 或更高版本）：

| 发布归档包 | 适配架构 | 构建说明 | 下载地址 |
|---|---|---|---|
| **`NexAir-1.3.6-Universal.zip`** | **Universal** (`arm64` + `x86_64`) | 最新正式版本 (Build 75) | [**立即下载最新版 (GitHub Releases)**](https://github.com/XideaDev/NexAir.APP/releases/latest) |

### 安装与首次启动指引：
1. 下载并解压 **`NexAir-1.3.6-Universal.zip`**；
2. 将 **`NexAir.app`** 拖移至你的 **`/Applications`**（应用程序）目录；
3. 若首次打开时系统 Gatekeeper 弹出“未受信任”或“已损坏”提示（本地 ad-hoc 签名特征），在终端执行一次解禁即可：
   ```bash
   xattr -dr com.apple.quarantine "/Applications/NexAir.app"
   ```
4. 双击启动 **NexAir**，智能欢迎向导与 MCP 环境将全自动初始化完成。

---

## 🛠️ 从源码构建 (Building from Source)

### 编译环境要求
- macOS 14.0+ 与 **Xcode 15+**（或已安装 Command Line Tools）
- Swift 5.10+ 工具链
- Go 1.22+（仅在重新编译自带的 `sing-box` 内核时需要）

### 本地构建指南

```bash
# 1. 克隆代码仓库
git clone https://github.com/XideaDev/NexAir.git
cd NexAir

# 2. 运行全量单元测试（517 个测试用例，100% 保证通过）
swift test

# 3. 编译打包生成 Universal 应用包
./scripts/build-app.sh

# 4. 验证签名与二进制架构
codesign --verify --deep --strict "dist/NexAir.app"
lipo -archs "dist/NexAir.app/Contents/MacOS/NexAir"

# 5. 覆盖安装到系统应用目录
rm -rf /Applications/NexAir.app && cp -R "dist/NexAir.app" /Applications/
```

### 标准自动化发版流程 (SOP)
```bash
# 执行原子化编译、双轨 Ed25519 签名、XML 校验与 GitHub Release 自动发布
./scripts/release.sh 1.3.6 "NexAir 1.3.6" "发布说明"
```

---

## ⌨️ 常用原生快捷键速查

| 快捷键 | 功能操作 |
|---|---|
| `⌘T` / `⌘N` | 新建标签页 / 新建窗口 |
| `⌘W` / `⌥⌘W` | 关闭当前标签页 / 关闭其他标签页 |
| `⇧⌘T` | 恢复最近关闭的标签页 |
| `⌘1` – `⌘8` / `⌘9` | 切换至第 1–8 个标签 / 切换至末尾标签 |
| `⇧⌘]` / `⇧⌘[` | 向右切换标签 / 向左切换标签 |
| `⌘L` | 聚焦智能胶囊地址栏 |
| `⌘R` / `⌥⌘R` | 刷新页面 / 强制无缓存硬刷新 |
| `⌘+` / `⌘-` / `⌘0` | 网页放大 / 网页缩小 / 恢复实际大小 |
| `⌥⌘I` / `⌥⌘U` | 审查元素 (Web Inspector) / 查看网页源代码 |
| `⌘\`` / `⌘·` | 快速循环切换窗口（兼容中英文输入法） |

---

## 📄 开源许可证与鸣谢 (License)

- **NexAir** 遵循 **[MIT 开源许可证](LICENSE)** 自由使用与二次分发。
- **Air Engine**: 内置了未修改的 **[SagerNet/sing-box](https://github.com/SagerNet/sing-box)** 二进制组件（遵循 GPL-3.0-or-later 许可协议）。完整许可与源码引证位于 `Contents/Resources/ThirdParty/sing-box/`。
- 由 **XideaDev** 核心工程团队倾力设计与研发维护。
