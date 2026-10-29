# LightTouch 简体中文版 · Android 0.0.11

Android 端的简体中文汉化版，基于 [blake-lucas/LightTouch](https://github.com/blake-lucas/LightTouch) v0.0.11 调试版（commit `3e0484d`）。

> 第三方汉化，非官方发布。以 GPL-2.0-or-later 分发，源码见仓库 `source/` 目录。

## 下载

| 文件 | 适用 | 大小 |
| --- | --- | --- |
| `LightTouch-0.0.11-android-arm64-zh.apk` | Android 10+ / ARM64 | 64.1 MB |

SHA-256：

```
463c388b87c588601642dc6cba2d0ac8938b7fd03558816797b063825ff79cc2  LightTouch-0.0.11-android-arm64-zh.apk
```

## 安装

1. **必须先卸载已安装的 LightTouch / Un1box**：本包用第三方自签名证书，签名与原版不同，无法覆盖安装。
2. 卸载会清空应用数据 —— 已准备的模拟设备、iOS 系统镜像、装过的应用都在应用私有目录（`allowBackup=false`）。
3. 安装后按引导选择机型与 iOS 版本；iPod touch 2G + iOS 3.1.3 最快，约一分钟。

## 改动

- 应用界面文案硬编码在 Kotlin（Jetpack Compose）中，`res/values/strings.xml` 只有 androidx/Material3 自带字符串，因此汉化在反编译后的 smali `const-string` 上完成。
- **602 条**文案、**706 处**替换、**179 个** smali 文件。
- 未改动：原生引擎（`libqemu-arm.so` 等，逐字节一致）、assets、AndroidManifest、包名；硬件名、格式串、`SharedPreferences` 键与枚举 `.name` 保留原样。
- 安装包体积 67.8 MB → 64.1 MB，仅因重新压缩。

## 验证情况

真机安装、启动正常；固件与商店下载正常，应用可正常安装；应用与游戏启动、运行正常；中文界面无缺字、无乱码（2026 年 10 月实测）。

汉化只改文案，未改动任何模拟逻辑；遇到功能问题请先对照原版复现。

## 源码

- 上游：<https://github.com/blake-lucas/LightTouch>（GPL-2.0-or-later）
- 对应源码 = 上游 tag `v0.0.11` + [`source/lighttouch-android-0.0.11-zh-patch.tar.gz`](https://github.com/18-boop/LightTouch-zh/blob/main/source/lighttouch-android-0.0.11-zh-patch.tar.gz)（179 个改动的 smali + 译文表 + 复现脚本）

iPhone、iPod touch、iPad 为 Apple Inc. 商标，本项目与 Apple 无关。
