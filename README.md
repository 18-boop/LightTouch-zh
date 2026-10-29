# LightTouch / Un1box 简体中文版

[简体中文](README.md) · [English](README.en.md)

基于 [blake-lucas/LightTouch](https://github.com/blake-lucas/LightTouch) 的简体中文汉化版，含 Windows 桌面版的一处网络问题修复。原项目是运行经典 iPhone OS / iOS（1–7）应用的模拟器，基于 qemu-ios。

- Android 端：汉化自 **v0.0.11 调试版**（commit `3e0484d`，即 `LightTouch-3e0484d-arm64-debug.apk`）
- Windows 端：汉化自 **v0.0.12 正式版**（`LightTouch-v0.0.12-Windows-x86_64.zip`），并修复了代理环境下的「无法连接旧版商店」问题

本仓库/发布页只提供**汉化改动与编译产物**，不包含 Apple 固件（IPSW），首次使用请按应用内引导自行下载。

## 下载

Android 与 Windows 分为两个 Release：

| 文件 | 说明 | 下载 |
| --- | --- | --- |
| `LightTouch-0.0.11-android-arm64-zh.apk` | Android 10+ / ARM64，简体中文 | [android-v0.0.11-zh](https://github.com/18-boop/LightTouch-zh/releases/tag/android-v0.0.11-zh) |
| `LightTouch-0.0.12-windows-x86_64-zh.zip` | Windows 10/11 64 位，简体中文 + 代理修复，解压即用 | [windows-v0.0.12-zh](https://github.com/18-boop/LightTouch-zh/releases/tag/windows-v0.0.12-zh) |
| `source/lighttouch-android-0.0.11-zh-patch.tar.gz` | Android 汉化改动：179 个改动的 smali + 译文表 + 回填/校验脚本 | 仓库内 |
| `source/lighttouch-v0.0.12-desktop-zh-source.tar.gz` | Windows 版完整改动源码（Rust 工程，含中文字体与代理修复） | 仓库内 |
| `SHA256SUMS.txt` | 全部产物的 SHA-256 | 仓库内 |

## 界面预览

Windows 桌面版（Android 版界面结构相同）：

| | |
|---|---|
| ![首页](docs/screenshots/windows-home.png) | ![设置](docs/screenshots/windows-settings.png) |
| ![添加设备](docs/screenshots/windows-devices.png) | ![从 IPSW 准备](docs/screenshots/windows-prepare.png) |
| ![旧版商店](docs/screenshots/windows-store.png) | |

## Android 安装说明

1. **必须先卸载已安装的 LightTouch/Un1box**：本包用自签名证书重新签名，与原版签名不同，无法覆盖安装。
2. 卸载会清空应用数据 —— 已准备的模拟设备、iOS 系统镜像、装过的应用都在应用私有目录（`allowBackup=false`），卸载即丢失。若手机上已有做好的设备，请先在一台干净设备上试。
3. 安装后首次进入按引导选择机型与 iOS 版本即可（iPod touch 2G + iOS 3.1.3 最快，约一分钟）。

## Windows 使用说明

1. 解压整个 zip 后运行 `lighttouch.exe`（不要只拷贝 exe，`resources/` 里有引擎与依赖 DLL）。
2. 若使用 Clash/V2Ray 等代理：**不需要打开系统代理**。程序会依次尝试环境变量 `HTTPS_PROXY` → Windows 系统代理 → 本机监听中的代理端口（7897/7890/7891/10809/10808）。
3. 代理端口不在上述范围时，可在启动前设置 `set HTTPS_PROXY=http://127.0.0.1:端口`。
4. 数据目录与原版一致，可继续使用已有的设备与设置。

## 改动说明

### Android（0.0.11）

- 应用的界面文案全部硬编码在 Kotlin（Jetpack Compose）中，`res/values/strings.xml` 只有 androidx/Material3 自带字符串；因此汉化在反编译后的 smali `const-string` 上完成。
- 共 **602 条**文案、**706 处**替换，涉及 **179 个** smali 文件。
- 未改动：原生引擎（`libqemu-arm.so` 等，逐字节一致）、assets、AndroidManifest、包名、签名以外的任何结构；硬件名（PowerVR/ARM Cortex/iPhone 4 等）、格式串、`SharedPreferences` 键与枚举 `.name` 一律保留原样，避免影响逻辑。
- 安装包体积变化（67.8 MB → 64.1 MB）仅因重新压缩。

### Windows（0.0.12）

- 桌面版是 Rust + egui 原生程序，字符串以 `&str` 常量编译进二进制，且 egui 自带字体不含汉字 —— 因此**必须从源码重编译**，不能像 APK 那样打补丁。
- 汉化：**602 条**对照表 + 20 余处长段落，共 **726 处**替换，覆盖 4 个 crate / 20 个 `.rs` 文件（界面、模型、目录层）。
- 字体：内嵌 Noto Sans SC 子集（OFL 授权，ASCII + CJK 基本区 + 全角标点，7.5 MB），在 `theme.rs` 中作为 fallback 追加到 proportional/medium/semibold 字族。
- **问题修复**：原版 `ureq` 客户端未配置代理，代理环境下访问旧版商店必然 `timeout: connect`。现改为三级解析（环境变量 → Windows 系统代理 → 本机常见代理端口自动探测），并遵守 `NO_PROXY`/`ProxyOverride`；商店报错文案附带排查指引。
- 保留英文：日志、CLI 输出、协议字段、Apple 专有名词（IPSW/IPA/boot ROM）、硬件规格。

## 已知限制

- Android 版未在真机上跑过完整流程（汉化后仅做静态校验：dex 解析、manifest 一致、原生库逐字节一致、v3 签名通过）。
- Windows 版已验证启动、退出与 8 个界面截图；未验证实际启动模拟设备、商店联网列表（需下载约 300 MB 固件）。
- 汉化只改文案，未改任何模拟逻辑；如遇功能问题请先对照原版复现。

## 源码与许可

- 上游：<https://github.com/blake-lucas/LightTouch>（GPL-2.0-or-later）
- Android 对应源码 = 上游 tag `v0.0.11` 源码 + 本包 `source/lighttouch-android-0.0.11-zh-patch.tar.gz` 中的 smali 改动与译文表
- Windows 对应源码 = 上游 `v0.0.12` 源码 + 本包 `source/lighttouch-v0.0.12-desktop-zh-source.tar.gz`（完整可编译工程）
- 中文字体：Noto Sans SC，SIL Open Font License 1.1（许可见包内 `NotoSansSC-OFL.txt`）

本项目同样以 **GPL-2.0-or-later** 分发，保留上游版权与许可声明。iPhone、iPod touch、iPad 为 Apple Inc. 商标，本项目与 Apple 无关。

## 校验

```bash
sha256sum -c SHA256SUMS.txt
```

Windows PowerShell：

```powershell
Get-FileHash .\LightTouch-0.0.12-windows-x86_64-zh.zip -Algorithm SHA256
```
