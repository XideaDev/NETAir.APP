<div align="center">
  <img src="assets/logo/image13.png" width="108" height="108" alt="NETAIR Icon" style="border-radius: 24px; box-shadow: 0 18px 40px -10px rgba(15,111,212,0.45);" />
  <h1 style="margin-top: 16px; font-size: 34px; font-weight: 800; letter-spacing: -0.02em;">NETAIR</h1>
  <p style="font-size: 16px; font-weight: 500; color: #6e6e73;">
    <strong>The Native macOS Multi-Sandbox Fingerprint Browser & AI Agent Runtime Base</strong>
  </p>
  <p style="max-width: 680px; font-size: 13.5px; line-height: 1.6; color: #86868b;">
    Engineered exclusively for macOS 14+ Sonoma & macOS 15+ Sequoia · Built on Genuine Apple WebKit · Absolute Zero JavaScript Injection · Fail-Closed Network Isolation · Designed in macOS 26+ Liquid Glass
  </p>
  <p>
    <a href="https://github.com/XideaDev/NETAir.APP/releases/latest"><img src="https://img.shields.io/badge/Release-v1.3.8-0F6FD4?style=for-the-badge&logo=apple" alt="Release v1.3.8" /></a>
    <img src="https://img.shields.io/badge/macOS-14.0%2B-1d1d1f?style=for-the-badge&logo=apple" alt="macOS 14.0+" />
    <img src="https://img.shields.io/badge/Architecture-Universal%20(arm64%20%2B%20x86__64)-34C759?style=for-the-badge" alt="Universal" />
    <img src="https://img.shields.io/badge/Engine-Native%20WebKit-007AFF?style=for-the-badge&logo=safari" alt="Native WebKit" />
    <img src="https://img.shields.io/badge/Invariants-Zero%20JS%20Injection-FF9500?style=for-the-badge" alt="Zero JS Injection" />
    <img src="https://img.shields.io/badge/License-Proprietary-gray?style=for-the-badge" alt="Proprietary" />
  </p>
  <p>
    <strong>English</strong> · <a href="README.zh-CN.md">简体中文</a> · <a href="#-quick-download--installation">Download</a> · <a href="#-system-architecture">Architecture</a> · <a href="#-built-for-ai-agents--mcp">AI Agent & MCP</a>
  </p>
</div>

---

## ⚡ What Makes NETAIR Fundamentally Different?

Market multi-account anti-detect browsers (AdsPower, Multilogin, Hubstudio, etc.) are built on modified Chromium distributions. They tamper with navigator properties before page loading via `Page.addScriptToEvaluateOnNewDocument`. 

Modern risk control systems (**Cloudflare Turnstile, DataDome, Akamai, Kasada**) easily detect prototype pollution, `Function.prototype.toString` discrepancies, and cross-realm iframe scope leaks.

```
Market Solutions (Chromium) ──► Injects JS Prototype Overrides ──► Detected by Cloudflare / DataDome
NETAIR Native WebKit        ──► System SPI & Env Invariants    ──► Genuine Safari indistinguishable from Apple Pool
```

### The Three Architectural Invariants of NETAIR:

1. 🛡️ **Absolute Zero JS Injection Law**:
   We **never** inject `WKUserScript`, and strictly prohibit altering `window`, `navigator`, `screen`, or `WebGL` prototypes in web browsing contexts. All hardware parameters, language stacks, and time zones take effect at the macOS system level (`-AppleLanguages`, `TZ`) and WebKit private SPI runtime bridging. Target sites observe a genuine Mac running Apple WebKit Safari.
2. 🔒 **Fail-Closed Network Isolation**:
   Each browser profile operates its own dedicated `Air Engine` process or isolated SOCKS5 outbound. If an outbound drops or DNS stalls, traffic terminates immediately with a localized deadlock. **Zero fallback to local direct connections** — preventing IP exposure and DNS leakage.
3. 🔐 **Zero-Plaintext Credential Vault**:
   Credentials, keys, and session identities are stored strictly in POSIX `0600` sandboxed vaults (`credentials.vault`) and macOS Keychain. Configs are transferred to child processes exclusively through `stdin` memory pipes (`F_SETNOSIGPIPE`).

---

## 📸 Interface Showcase

<div align="center">
  <img src="assets/screenshots/dashboard-1.3.6.png" width="94%" alt="NETAIR Liquid Glass Dashboard" style="border-radius: 14px; box-shadow: 0 20px 50px rgba(0,0,0,0.25);" />
  <p><em>NETAIR Modern Manager Console · Dual-Mode Liquid Glass Dashboard with 6-Quadrant Bento Topology</em></p>
</div>

<br />

<div align="center">
  <table width="100%" style="border: none;">
    <tr>
      <td width="50%" align="center" style="border: none;">
        <img src="assets/screenshots/browser-multi-1.3.6.png" width="100%" alt="1:1 Safari Chrome Capsule Bar" style="border-radius: 10px;" />
        <br /><strong>1:1 Safari Chrome Capsule Tab Bar</strong>
      </td>
      <td width="50%" align="center" style="border: none;">
        <img src="assets/screenshots/browser-single-1.3.6.png" width="100%" alt="Single Tab Bar with Vector Flag" style="border-radius: 10px;" />
        <br /><strong>Single-Tab Auto-Compact Mode with Vector Flag</strong>
    </tr>
  </table>
</div>

---

## 🖥️ System Architecture

NETAIR adopts an ultra-reliable **Single Binary, Multi-Process** orchestrated architecture:

```
                      ┌──────────────────────────────────────────────┐
                      │          NETAir App (Single Binary)          │
                      └───────┬──────────────────────────────┬───────┘
                              │                              │
         [Default Launch]     │                              │ [run --profile <ID>]
                              ▼                              ▼
             ┌────────────────────────────────┐  ┌────────────────────────────────┐
             │       NETAIR ManagerGUI         │  │         NETAir Runner          │
             │   (SwiftUI Modern Dashboard)   │  │   (Isolated AppKit Process)    │
             └────────────────────────────────┘  └───────┬────────────────┬───────┘
                                                         │                │
                                                         ▼                ▼
                                            ┌──────────────────┐  ┌──────────────┐
                                            │   NETAirWebKit   │  │AirEngine(PID)│
                                            │(WKWebView Engine)│  │(Local Port)  │
                                            └────────┬─────────┘  └──────────────┘
                                                     ▼
                                            ┌──────────────────┐
                                            │NETAirObjCSupport │
                                            │(Dynamic SPI Hook)│
                                            └────────┬─────────┘
                                                     ▼
                                            ┌──────────────────┐
                                            │   NETAirCore     │
                                            │(Sandbox & Vault) │
                                            └──────────────────┘
```

### Module Responsibilities & System Boundaries

| Module | Location | Primary Responsibilities |
|---|---|---|
| **`NETAirCore`** | `Sources/NETAirCore` | Sandbox metadata, `PrivateVaultCredentialStore` (0600), 34 authentic Mac device models, `BrowsingHistoryStore`, `ProfileLock`, and `MCPServerRegistry`. |
| **`NETAirNetwork`** | `Sources/NETAirNetwork` | Node parser (VLESS Reality, VMess, Trojan, SS2022, SOCKS5), Air Engine configuration synthesizer, port dispatch, DNS leak mitigation, and fail-closed routing. |
| **`NETAirObjCSupport`** | `Sources/NETAirObjCSupport` | Dynamic Objective-C runtime bridge, XOR-decoded WebKit SPI symbols, signature validation, method swizzling for `NSScreen`, and C font allowlist configuration. |
| **`NETAirWebKit`** | `Sources/NETAirWebKit` | `WKWebViewConfiguration` assembly, physical `WKWebsiteDataStore` binding, `SPIPrivacyAdapter` privacy orchestration, and display traits simulation. |
| **`NETAir` (App)** | `Sources/NETAir` | Executive assembly: SwiftUI Manager dashboard (`ManagerGUI/`), 1:1 Safari Chrome capsule tab bar (`Browser/`), Runner process main, and IPC bridge server. |

---

## ✨ Features & Core Capabilities

### 1. 34 Authentic Mac Hardware Presets
Simulates authentic Apple Silicon (M1/M2/M3/M4) and Intel Mac device traits:
- **MacBook Pro 16" & 14"** (Liquid Retina XDR, Display P3, HDR/EDR headroom, Notch geometry)
- **MacBook Air 15" & 13"** (Display P3, SDR, Notch / Traditional aspect ratio)
- **iMac 24" 4.5K Retina** (4480×2520 @2x, Display P3)
- **Mac Studio with Studio Display** (5120×2880 5K Retina, Display P3)
- **Mac Pro with Pro Display XDR** (6016×3384 6K Retina, Display P3, HDR)

### 2. Auto Region & Locale Following
Select an outbound node anywhere across the globe. NETAIR measures the true exit IP and synchronizes:
- **Language Stack** (`Accept-Language`, `navigator.languages`)
- **System Time Zone** (`TZ` environment variable, WebKit Date engine)
- **Format Locales** (Currency, numbers, calendars)

### 3. Native Font Allowlist Filtering
Blocks third-party font enumeration attacks at WebKit core without JS injection. Retains clean macOS baseline system fonts (including CJK fonts) while filtering language packs to match target locales.

### 4. Built-in All-Protocol Air Engine
Zero external proxy tools required. Natively parses subscription URLs or raw links:
- `VLESS` (Reality / uTLS / Vision / gRPC / WebSocket)
- `VMess` (WebSocket / gRPC / TLS)
- `Trojan`
- `Shadowsocks` & `SS2022` (2022-blake3-aes-128-gcm / 2022-blake3-aes-256-gcm)
- `AnyTLS`, `SOCKS5`, `HTTP`

### 5. Authentic Safari 1:1 Capsule Tab Bar
- **Pixel-Perfect Safari Capsule Tabs**: 30pt height, continuous squircle rounding (10.5pt), automatic full-width track distribution (`maxWidth: .infinity`), and hover divider avoidance.
- **26pt Vector Flag Capsule**: Displays authentic vector flags with instant popover outbound inspection.
- **Zero-Polling Multi-Tab Architecture**: Lifecycle-event driven updates eliminate UI freezes even with 20+ active tabs.

---

## 🤖 Built for AI Agents & MCP

NETAIR is built ground-up as an autonomous runtime for AI Agents (Claude Code, OpenAI Operator, Codex, Cursor, OpenCode, PI-Desktop).

### Full-Ecosystem MCP Server Auto-Registration
Launching NETAIR automatically registers the **`netair`** MCP server into all major agent runtimes:
- **Standard Global MCP**: `~/.agents/servers/netair.json`
- **Project-Level MCP**: `{Workspace}/.agents/servers/netair.json`
- **Codex Config**: Automatically injected into `~/.codex/config.toml`
- **OpenCode Config**: Automatically merged into `~/.config/opencode/opencode.json`
- **Claude Desktop & Cursor**: Registered in `claude_desktop_config.json` & `~/.cursor/mcp.json`
- **CLI Symlink**: Installed to `~/.local/bin/netair`

```bash
# Verify NETAIR MCP stdio response
echo '{"jsonrpc":"2.0","id":1,"method":"initialize","params":{"protocolVersion":"2024-11-05","capabilities":{},"clientInfo":{"name":"test","version":"1.0"}}}' | /Applications/NETAir.app/Contents/MacOS/NETAir mcp
```

### Agent CLI Capabilities

```bash
NX="/Applications/NETAir.app/Contents/MacOS/NETAir"

# 1. Query available sandbox profiles
"$NX" agent spaces

# 2. Open page in isolated background sandbox
"$NX" agent open "Workspace US" https://example.com --background

# 3. Capture semantic accessibility snapshot (tagged elements: e1, e2...)
"$NX" agent snapshot "Workspace US"

# 4. Fill form inputs & trigger native AppKit clicks
"$NX" agent fill "Workspace US" e1 "user@example.com"
"$NX" agent click "Workspace US" e2

# 5. Native coordinate click for canvas & bot-detection puzzles
"$NX" agent click-coord "Workspace US" 420 310

# 6. Capture full-resolution viewport snapshot
"$NX" agent screenshot "Workspace US" /tmp/view.png
```

---

## 📥 Quick Download & Installation

Download the official standalone application (Universal binary for **Apple Silicon** & **Intel Macs**, requiring **macOS 14 Sonoma** or later):

| Release Asset | Architecture | Description | Download |
|---|---|---|---|
| **`NETAir-1.3.8-Universal.zip`** | **Universal** (`arm64` + `x86_64`) | Latest Production Build (Build 91) | [**Download Latest (GitHub Releases)**](https://github.com/XideaDev/NETAir.APP/releases/latest) |

### Installation Steps:
1. Download and extract **`NETAir-1.3.8-Universal.zip`**;
2. Move **`NETAir.app`** to your **`/Applications`** folder;
3. If macOS Gatekeeper displays an unverified warning on initial launch (ad-hoc builds):
   ```bash
   xattr -dr com.apple.quarantine "/Applications/NETAir.app"
   ```
4. Launch **NETAIR** — the welcome setup and MCP registration will self-initialize.

---

## 🛠️ Building from Source

### Prerequisites
- macOS 14.0+ with **Xcode 15+** (or Command Line Tools)
- Swift 5.10+ Toolchain
- Go 1.22+ (only if compiling custom `Air Engine` core helpers)

### Build Instructions

```bash
# 1. Clone repository
git clone https://github.com/XideaDev/NETAir.git
cd NETAir

# 2. Execute full automated test suite (517 unit tests, 100% pass guarantee)
swift test

# 3. Build Universal distribution app bundle
./scripts/build-app.sh

# 4. Verify code signature & binary architectures
codesign --verify --deep --strict "dist/NETAir.app"
lipo -archs "dist/NETAir.app/Contents/MacOS/NETAir"

# 5. Install to /Applications
rm -rf /Applications/NETAir.app && cp -R "dist/NETAir.app" /Applications/
```

### Standard Automated Release (SOP)
```bash
# Executes atomic build, dual Ed25519 signing, XML verification & GitHub release deployment
./scripts/release.sh 1.3.8 "NETAIR 1.3.6" "Production Release Notes"
```

---

## ⌨️ Essential Keyboard Shortcuts

| Shortcut | Description |
|---|---|
| `⌘T` / `⌘N` | New Tab / New Window |
| `⌘W` / `⌥⌘W` | Close Current Tab / Close Other Tabs |
| `⇧⌘T` | Reopen Last Closed Tab |
| `⌘1` – `⌘8` / `⌘9` | Switch to Tab 1–8 / Last Tab |
| `⇧⌘]` / `⇧⌘[` | Next Tab / Previous Tab |
| `⌘L` | Focus Smart Capsule Address Bar |
| `⌘R` / `⌥⌘R` | Reload Page / Hard Reload (Bypass Cache) |
| `⌘+` / `⌘-` / `⌘0` | Zoom In / Zoom Out / Actual Size |
| `⌥⌘I` / `⌥⌘U` | Inspect Element / View Page Source |
| `⌘\`` / `⌘·` | Fast Window Cycling (EN / CN IME compatible) |

---

## 📄 License & Acknowledgments

- **NETAIR** is proprietary software. All rights reserved.
- **Air Engine**: Built upon an unmodified helper core of **[SagerNet/sing-box](https://github.com/SagerNet/sing-box)** (GPL-3.0-or-later) for low-level proxy routing. Full license notices are bundled inside `Contents/Resources/ThirdParty/sing-box/`.
- Designed and engineered with passion by the **XideaDev** Core Team.
