# Windows 桌面版汉化的构建说明

本文件说明如何从上游 v0.0.12 源码构建 `LightTouch-v0.0.12-windows-x86_64-zh.zip`。

## 改动一览（相对上游 `v0.0.12`）

| 文件 | 改动 |
| --- | --- |
| `crates/lighttouch/src/ui/*.rs` | 界面文案汉化（359 条） |
| `crates/lighttouch/src/ui/theme.rs` | 内嵌 Noto Sans SC 子集并加入字体 fallback |
| `crates/lighttouch/assets/fonts/NotoSansSC-LightTouch.ttf` | 新增：中文字体子集（7.5 MB） |
| `crates/lighttouch/assets/fonts/NotoSansSC-LICENSE-OFL.txt` | 新增：字体许可 |
| `crates/lighttouch-model/src/*.rs` | 状态/商店/准备进度等文案汉化（151 条） |
| `crates/lighttouch-catalog/src/catalog.rs` | 测试徽章、固件摘要、耗时提示等汉化 |
| `crates/lighttouch-catalog/src/{store,ipa,download,guest_tools}.rs` | 商店与 IPA 校验提示汉化 |
| `crates/lighttouch-catalog/Cargo.toml` | `ureq` 启用 `win-system-proxy` 特性 |
| `crates/lighttouch-catalog/src/download.rs` | 新增代理解析：环境变量 → Windows 系统代理 → 本机常见代理端口（7897/7890/7891/10809/10808） |
| `.cargo/config.toml` | 链接器改为 `x86_64-w64-mingw32-gcc`（MSYS2 命名），并加入 crates 镜像配置（可删） |

## 构建环境

- Windows 10/11 x64
- MSYS2（`C:\msys64`）：`mingw-w64-x86_64-gcc`（16.2）、`mingw-w64-x86_64-openssl`、`mingw-w64-x86_64-pkgconf`
- Rust：`rustup` + `stable-x86_64-pc-windows-gnu`（rustc 1.99.1）
- OpenSSL 静态链接（使用 MSYS2 的 `libssl.a` / `libcrypto.a`）

## 构建

```powershell
$env:PATH = "C:\msys64\mingw64\bin;$env:USERPROFILE\.cargo\bin;" + $env:PATH
$env:OPENSSL_DIR='C:\msys64\mingw64'
$env:OPENSSL_STATIC='1'
$env:OPENSSL_INCLUDE_DIR='C:\msys64\mingw64\include'
$env:OPENSSL_LIB_DIR='C:\msys64\mingw64\lib'

cargo build --release -p lighttouch
```

产物：`target\release\lighttouch.exe`（约 40.4 MB）。

## 打包

把 `lighttouch.exe` 覆盖进官方 `LightTouch-v0.0.12-Windows-x86_64\` 目录（保留其 `resources/`、DLL），再把字体许可 `NotoSansSC-OFL.txt` 一并放入，压缩即可。

## 字体子集（可选，已生成）

若需重新生成中文字体子集（例如换用完整字符集）：

```bash
pip install fonttools
python -m fontTools.varLib.instancer NotoSansSC-var.ttf wght=400 -o NotoSansSC-Regular.ttf
python -m fontTools.subset NotoSansSC-Regular.ttf \
  --unicodes=U+0020-007E,U+00A0-00FF,U+2000-206F,U+3000-303F,U+4E00-9FFF,U+FF00-FFEF \
  --output-file=NotoSansSC-LightTouch.ttf --layout-features=* --name-IDs=* --no-hinting
```

## 验证

```powershell
# 截图钩子（程序自带，用于无头检查界面）
$env:LIGHTTOUCH_UI_SHOT="D:\shot.png"
$env:LIGHTTOUCH_UI_PAGE="settings"   # settings / store / network / add / licenses
.\lighttouch.exe
```
