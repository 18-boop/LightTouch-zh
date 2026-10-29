# LightTouch 简体中文版 · Windows 0.0.12

Windows 桌面版的简体中文汉化版，基于 [blake-lucas/LightTouch](https://github.com/blake-lucas/LightTouch) v0.0.12 正式版，并修复了代理环境下无法访问「旧版商店」的问题。

> 第三方汉化，非官方发布。以 GPL-2.0-or-later 分发，源码见仓库 `source/` 目录。

## 下载

| 文件 | 适用 | 大小 |
| --- | --- | --- |
| `LightTouch-0.0.12-windows-x86_64-zh.zip` | Windows 10/11 64 位，解压即用 | 56.7 MB |

SHA-256：

```
92d8d73f12f296a2fbbf89858af5a043f6540f639edcadb2efa47c411642e95b  LightTouch-0.0.12-windows-x86_64-zh.zip
```

## 使用

1. **解压整个 zip** 后运行 `lighttouch.exe`，不要只拷贝 exe —— `resources/` 里有引擎与依赖 DLL。
2. 数据目录与原版一致，可继续使用已有的设备与设置；这是绿色包，与原版目录互不影响。

## 问题修复：代理环境下商店连不上

**现象**：Clash 等代理开着但系统代理开关关闭时，打开应用详情必然报「无法连接旧版商店，请检查网络。（timeout: connect）」。

**原因**：上游的 `ureq` HTTP 客户端没有配置任何代理，只走直连。

**修复**：代理按三级解析 ——

1. 环境变量 `HTTPS_PROXY` / `HTTP_PROXY` / `ALL_PROXY`；
2. Windows 系统代理（遵守 `NO_PROXY` / `ProxyOverride`）；
3. 自动探测本机常见代理端口：`7897`、`7890`、`7891`、`10809`、`10808`。

因此**不必打开系统代理**，只要代理程序在跑就会被自动使用。端口不在上述范围时，可用 `set HTTPS_PROXY=http://127.0.0.1:端口` 启动。IPSW、boot ROM、商店与下载共用同一个客户端，下载功能同样受益。

## 改动

- 桌面版是 Rust + egui 原生程序，字符串以 `&str` 常量编译进二进制，且 egui 自带字体不含汉字 —— 因此从源码重新编译。
- **602 条**对照表 + 20 余处长段落，共 **726 处**替换，覆盖 4 个 crate / 20 个 `.rs` 文件。
- 内嵌 Noto Sans SC 子集（OFL 授权，ASCII + CJK 基本区 + 全角标点，7.5 MB），作为 proportional / medium / semibold 字族的 fallback。
- 保留英文：日志、CLI 输出、协议字段、Apple 专有名词（IPSW/IPA/boot ROM）、硬件规格。

## 验证情况

启动、退出正常；固件与商店下载正常（代理开启、Windows 系统代理关闭的环境下同样可用，说明代理自动探测生效）；应用与游戏启动、运行正常；中文界面无方块字（2026 年 10 月实测）。

汉化只改文案与代理逻辑，未触碰模拟引擎。

## 源码

- 上游：<https://github.com/blake-lucas/LightTouch>（GPL-2.0-or-later）
- 对应源码 = 上游 `v0.0.12` + [`source/lighttouch-v0.0.12-desktop-zh-source.tar.gz`](https://github.com/18-boop/LightTouch-zh/blob/main/source/lighttouch-v0.0.12-desktop-zh-source.tar.gz)（完整可编译 Rust 工程，含字体与代理修复）
- 字体：Noto Sans SC，SIL Open Font License 1.1

iPhone、iPod touch、iPad 为 Apple Inc. 商标，本项目与 Apple 无关。
