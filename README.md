# 🚀 You-Can-Copy-And-Paste v2.0

![Version](https://img.shields.io/badge/Version-2.0.0-blue.svg?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Windows_10%20%7C%2011-lightgrey.svg?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-AutoHotkey_v2-green.svg?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-orange.svg?style=for-the-badge)

> 突破在线评测沙箱与严苛编辑器的终极“物理级”复制粘贴工具。

## 📖 简介

在部分在线代码评测系统（如基于 Monaco Editor、CodeMirror 深度定制的沙箱）中，常规的 `Ctrl+V` 或系统剪贴板注入经常会被 JavaScript 拦截或导致格式彻底崩坏。

**You-Can-Copy-And-Paste v2.0** 采用降维打击策略：**放弃系统级粘贴，转而模拟真实人类的物理键盘输入**。它能将你的剪贴板内容以绝对安全的文本模式逐行“敲”进任何顽固的编辑器中，让你重新夺回复制粘贴的自由。

---

## ✨ 核心特性

- 🛡️ **物理级注入引擎**：使用底层的 `SendEvent {Text}` 模式，完美绕过前端 JS 拦截。
- 🤖 **仿人类延迟算法 (Delay Algorithm)**：内置三种波动延迟模式（Fast / Standard / Conservative），模拟真实人类输入节奏。
- 🧹 **智能文本消毒**：自动清除导致换行错乱的 `\r`，智能转换 Tab 为空格，处理不可见字符（NBSP），确保代码格式完美保留。
- ⏱️ **强力时序控制**：通过精确的 `ClipWait` 确保复制不丢数据，通过物理回车后的 `Sleep` 阻塞等待，完美适配沙箱的自动缩进（Auto-indent）渲染。
- 🎨 **现代化 GUI 设置**：提供直观的欢迎界面与设置面板，支持一键开机自启（绿色写入注册表 HKCU）。
- 🔌 **热键防冲突设计**：内置物理修饰键强制释放机制，防止键盘粘连。

---

## 🚀 快速开始

### 运行环境
- 操作系统：Windows 10 / Windows 11
- 运行依赖：[AutoHotkey v2.0+](https://www.autohotkey.com/) （如使用编译后的 `.exe` 则无需安装）

### 启动方式
1. 下载最新版本的压缩包并解压。
2. 双击运行 `you-can-copy-and-paste-v2.ahk` 或编译后的可执行文件。
3. 任务栏右下角出现绿色的 **H** 托盘图标即代表运行成功！

---

## ⌨️ 快捷键指南

| 快捷键 | 功能 | 详细说明 |
| :--- | :--- | :--- |
| `Ctrl` + `Insert` | **强力复制** | 清空剪贴板并重新捕获选中文本，具有底层防空等待机制。 |
| `Shift` + `Insert` | **强力注入 (粘贴)** | 开始将剪贴板内容逐行转化为物理按键输入目标窗口。 |
| `Shift` + `Esc` | **紧急停止** | 遇到失控情况或输入法冲突时，一键重载并终止所有注入任务。 |

> 💡 **Tip:** 注入过程中，屏幕鼠标指针处会实时显示注入进度（如：`正在注入: 行 45/100`）。

---

## ⚙️ 进阶配置

右键点击系统托盘图标，选择 **设置 (Settings)** 即可打开配置面板。支持动态调整以下参数：

### 延迟模式 (Delay Modes)
为了应对不同网络环境和不同性能的在线沙箱，工具提供了三种预设的输入节奏：

1. ⚡ **Fast (极速模式)**: `10ms ± 5ms` - 适用于本地高性能编辑器或不限制输入频率的系统。
2.  متع **Standard (标准模式)**: `30ms ± 15ms` - 最均衡的模式，适配 90% 以上的主流在线沙箱。
3. 🐢 **Conservative (保守模式)**: `50ms ± 25ms` - 适用于网络延迟较高或响应极慢的老旧网页。

> 所有的配置都将以纯绿色模式保存在同目录下的 `config.ini` 文件中。

---

## 🏗️ 架构设计

本项目采用高度模块化的面向对象设计 (OOP)：
- `Injection_Engine`: 核心注入逻辑与渲染等待
- `Delay_Algorithm`: 随机波动延迟算法生成器
- `Clipboard_Processor`: 数据清洗与环境隔离
- `Hotkey_Handler`: 全局热键劫持与安全释放
- `Config_Manager` & `UI_Manager`: 数据持久化与交互界面

详细的系统架构图与数据流向，请参考开发文档：[`design.md`](./design.md)。

---

## ⚠️ 免责声明

本工具仅供学习交流与自动化输入效率提升使用。请遵守各在线评测平台（OJ）、考试系统及企业沙箱的使用规范，切勿用于作弊或违反平台规则的行为。因不当使用造成的账号封禁或其他后果，由使用者自行承担。

---
*Developed with ❤️ using AutoHotkey v2.*
