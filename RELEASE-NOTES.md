# LightTouch 简体中文版 · Android 0.0.11 / Windows 0.0.12

基于 [blake-lucas/LightTouch](https://github.com/blake-lucas/LightTouch) 的**第三方简体中文汉化版**，并修复了 Windows 桌面版在代理环境下无法访问「旧版商店」的问题。

> 这不是官方发布。原始项目与引擎版权归原作者，本版仅做界面汉化与一处网络修复，以 GPL-2.0-or-later 分发。

## 下载

| 文件 | 适用 | 大小 |
| --- | --- | --- |
| `LightTouch-0.0.11-android-arm64-zh.apk` | Android 10+ / ARM64 | 64.1 MB |
| `LightTouch-0.0.12-windows-x86_64-zh.zip` | Windows 10/11 64 位，解压即用 | 56.7 MB |

SHA-256：

```
463c388b87c588601642dc6cba2d0ac8938b7fd03558816797b063825ff79cc2  LightTouch-0.0.11-android-arm64-zh.apk
92d8d73f12f296a2fbbf89858af5a043f6540f639edcadb2efa47c411642e95b  LightTouch-0.0.12-windows-x86_64-zh.zip
```

## Android 版

- 汉化自 v0.0.11 调试版（commit `3e0484d`）：602 条文案、706 处替换、179 个 smali 文件。
- **必须卸载已安装的原版**：本包用第三方自签名证书，签名不同无法覆盖安装；卸载会清空应用数据（已准备的模拟设备、iOS 镜像、已装应用），请谨慎。
- 安装后按引导选择机型与 iOS 版本即可（iPod touch 2G + iOS 3.1.3 最快）。

## Windows 版

- 汉化自 v0.0.12 正式版：602 条对照 + 长段落，共 726 处替换，覆盖 4 个 crate；内嵌 Noto Sans SC 子集作为 egui 字体。
- **修复**：原版 HTTP 客户端未配代理，代理环境下访问旧版商店必然 `timeout: connect`。现支持环境变量 `HTTPS_PROXY`、Windows 系统代理，以及自动探测本机常见代理端口（7897 / 7890 / 7891 / 10809 / 10808）——Clash 开着但系统代理没开也能用。
- 解压整个 zip 后运行 `lighttouch.exe`，不要只拷贝 exe。

## 未改动 / 验证情况

- 未改动：模拟引擎、原生库、assets、包名、manifest；硬件名、格式串、配置键、日志、协议字段保留原文。
- 已验证（2026 年 10 月实测）：两端安装与启动正常，固件与商店下载正常，应用与游戏启动运行正常，中文界面无缺字、乱码、方块字。

## 源码

- 对应源码见仓库 `source/` 目录：
  - `lighttouch-android-0.0.11-zh-patch.tar.gz`：179 个改动的 smali + 译文表 + 复现脚本
  - `lighttouch-v0.0.12-desktop-zh-source.tar.gz`：完整可编译 Rust 工程（含中文字体与代理修复）
- 上游：<https://github.com/blake-lucas/LightTouch>（GPL-2.0-or-later）
- 字体：Noto Sans SC，SIL Open Font License 1.1

## 致谢

原作者 [blake-lucas](https://github.com/blake-lucas) 与 [samhenrigold](https://github.com/samhenrigold)（LightTouchMac / qemu-ios）。iPhone、iPod touch、iPad 为 Apple Inc. 商标，本项目与 Apple 无关。
