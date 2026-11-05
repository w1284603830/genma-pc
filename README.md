# genma-pc · 幻魔鬼武者 PC 移植运行层

《幻魔鬼武者》Xbox 日版的非官方 Windows 原生移植运行层，基于 [Spark198rus/black-pc](https://github.com/Spark198rus/black-pc) 开发，提供启动器、输入增强及图形修复。

**游戏功能版本：v0.2.6**  
**公开包修订：r2（2026-10-17）**

## 源码在哪里

本项目的运行层、启动器、适配工具和测试源码发布在 [v0.2.6 Release 附件](https://github.com/w1284603830/genma-pc/releases/tag/v0.2.6)中。**当前 `main` 分支仅包含本 README；直接克隆仓库、下载主分支 ZIP，或下载该 Release 的 GitHub 自动生成 `Source code (zip/tar.gz)`，均不能取得运行层源码。**

| 用途 | 下载内容 |
|---|---|
| 开发、编译或研究 | [独立源码包：genma-pc_v0.2.6_._20261017_r2.zip](https://github.com/w1284603830/genma-pc/releases/download/v0.2.6/genma-pc_v0.2.6_._20261017_r2.zip) |
| 使用已编译程序 | [便携程序包：genma-pc.packge_r2.zip](https://github.com/w1284603830/genma-pc/releases/download/v0.2.6/genma-pc.packge_r2.zip) |
| 阅读玩家安装说明 | [genma-pc.packge_r2.txt](https://github.com/w1284603830/genma-pc/releases/download/v0.2.6/genma-pc.packge_r2.txt) |

独立源码包解压后，工程根目录为 `genma-pc-v0.2.6`。便携程序包也内嵌相同源码 ZIP，位置为 `GenmaPC/源码/genma-pc_v0.2.6_开源分享_20261017_r2.zip`。外部附件名与包内源码文件名不同，请按用途选择。

## 项目范围

- 发布的源码为移植运行层、启动器及相关工具，不是 CAPCOM 原始游戏开发源码。
- 标准编译不需要游戏文件、Xbox BIOS 或 Xbox SDK；运行需要自行准备受支持版本的完整 Xbox 游戏文件。
- 本次源码包和便携包未附 `default.xbe`、ISO/XISO、BIOS 或个人存档。运行层的地址表与补丁适用于特定游戏版本，不能直接作为其他 Xbox 游戏的通用移植。
- 原游戏内容、角色、名称、商标及第三方素材的权利不因运行层的开源许可而转移。

## 功能与修复

- 针对《幻魔鬼武者》的图形、音频、手柄与键盘输入适配及启动器。
- 按镜头方向移动的左摇杆、攻击软锁定、架势四向闪避。
- 包含背景切换延迟、侧闪方向、争夺绿魂时的贴图上传掉帧、鬼战术后输入锁，以及人物阴影相关修复。

这些功能及修复的实际验证范围见源码包内 `docs/验证范围.md`；不代表全部游戏场景和硬件均已验证。

## 运行方式

项目加载用户自己的 Xbox 游戏 XBE，直接执行其中的 x86 游戏代码，并通过 HLE（高层模拟）适配系统、图形与音频接口。

本版没有使用 xboxrecomp 将整个游戏转换为 C 代码。

## 从源码编译

先下载并解压上述**独立源码包**，再阅读解压目录中的 `BUILD.md`。该文件不在当前 `main` 分支中。

开发环境：

- Windows 10/11。
- Visual Studio 2022 或 Build Tools 2022，安装 C++ 桌面开发、MSVC x86 工具及 Windows SDK。
- CMake 3.24+。
- .NET Framework 4 的 `csc.exe`，用于 C# WinForms 启动器。

运行层固定为 Win32/x86，使用 C++20。建议源码和输出目录使用英文路径。标准编译无需 Python；只有可选分析工具需要 `tools/requirements.txt` 中的 Capstone。

在解压后的 `genma-pc-v0.2.6` 工程根目录打开 PowerShell：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\build.ps1 -BuildDirectory C:\GenmaBuild
```

脚本将生成 `C:\GenmaBuild\package`，包含 `genma.exe`、`genmart.dll`、`GenmaLauncher.exe` 和许可文档。然后按 `BUILD.md` 配置自己的完整游戏文件，运行启动器。支持的 XBE SHA256、手动编译步骤和可选分析工具说明均见该文件。

不依赖游戏资源的专项测试入口：

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File .\test.ps1 -BuildDirectory C:\GenmaTests
```

GPU 测试需要可用的 D3D9 图形设备。包内 `docs/构建验证结果.txt` 记录维护者在 2026-10-17 的构建及测试结果；不代表所有开发环境均已验证。

## 源码包目录

以下路径均位于解压后的源码包内，当前不在 `main` 分支中：

| 路径 | 内容 |
|---|---|
| `loader/src/`、`loader/stub/` | C/C++ 运行层、HLE、图形、音频、输入和游戏专用修复 |
| `loader/CMakeLists.txt`、`loader/CMakePresets.json` | Win32 构建配置 |
| `launcher/` | C# WinForms 启动器、构建脚本及可选图片资源 |
| `scripts/Find-CMake.ps1` | CMake 定位与调用辅助脚本 |
| `tools/` | 可选 XBE 分析、函数表生成及公开包检查工具 |
| `loader/tests/`、`test.ps1` | 专项测试及可选诊断 |
| `build.ps1`、`BUILD.md` | 构建入口与编译文档 |
| `docs/` | 上游来源、修改清单、排查记录和验证范围 |
| `LICENSE`、`THIRD_PARTY_NOTICES.txt` | 完整许可与第三方来源说明 |

## 开发、验证与反馈

开发过程中使用了 Claude、ChatGPT 与 Codex 辅助编写代码、分析问题及整理文档，并结合实际试玩和玩家反馈逐步修正问题。

目前已进行代表场景验证与专项回归，尚未完成全部游戏流程和所有硬件环境验证。仍有零星帧时间抖动；ROOM212 使用静态背景兜底。详细限制见源码包内 `docs/验证范围.md`。

反馈问题时请提供项目版本及包装修订、游戏版本、场景与复现步骤、预期和实际表现、系统与硬件配置，以及相关日志。

## 许可与致谢

本项目以 Spark198rus 的 [black-pc](https://github.com/Spark198rus/black-pc) 为基础。源码包记录的上游导入提交为 [`4c84944fc390194ff8e0a18bb723f48594bfbcf0`](https://github.com/Spark198rus/black-pc/tree/4c84944fc390194ff8e0a18bb723f48594bfbcf0)；该提交的 [README「Legal」](https://github.com/Spark198rus/black-pc/blob/4c84944fc390194ff8e0a18bb723f48594bfbcf0/README.md#legal) 明确采用 GPL 第 2 版或任何后续版本。

移植代码沿用 **GPL-2.0-or-later**，本项目新增代码及新编写文档的声明见源码包。应保留已有版权、修改及许可信息，并遵守所分发版本适用的源码提供要求。

**完整 `LICENSE`、`THIRD_PARTY_NOTICES.txt` 及上游 README 副本位于上述源码包中；便携程序包也附有许可与第三方说明。当前 `main` 分支未单独收录这些文件。** 上游 GPL 全文亦可在[导入提交的 LICENSE](https://github.com/Spark198rus/black-pc/blob/4c84944fc390194ff8e0a18bb723f48594bfbcf0/LICENSE)中阅读。

第三方说明列出了 black-pc 的直接继承关系、XbSymbolDatabase 与 Capstone 的可选分析用途，以及 Cxbx-Reloaded、xemu、nxdk 等行为参考；具体范围以包内说明及各文件原有声明为准。AI 辅助工具的致谢不替代第三方代码的版权和许可通知。

本项目与 CAPCOM、Microsoft 无隶属或官方授权关系。代码许可不授予原游戏内容、第三方作品或商标的权利；启动器非代码图片的来源说明见包内文档。

## 当前 r2 附件校验

| 附件名 | 字节数 | SHA256 |
|---|---:|---|
| `genma-pc_v0.2.6_._20261017_r2.zip` | 2,478,655 | `8d9b3e031c7b95a86e3d5ad32399ba4d340a23755fb25ccf2e16f327a6400f95` |
| `genma-pc.packge_r2.zip` | 5,357,752 | `4dc1aa2f53a2ae001a1290f6e83ce420d9bcf980c066e7510e938539d12b56b8` |

源码包和便携包内的 `SHA256SUMS.txt` 可用于逐文件校验。若附件以后更新，应同时更新文件名、大小和校验值。
