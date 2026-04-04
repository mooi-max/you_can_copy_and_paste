# Task 8.3 实现设置界面 - 验证文档

## 任务概述
实现 `ShowSettingsWindow()` 方法，创建完整的设置界面 GUI。

## 实现内容

### 1. 设置界面组件
已实现以下所有组件：

#### ✅ 延迟模式下拉菜单
- 包含三个选项：Fast、Standard、Conservative
- 自动加载当前配置的延迟模式
- 切换模式时自动更新基础延迟和波动范围字段

#### ✅ 基础延迟输入框
- 带标签 "基础延迟 (Base Delay)："
- 数字输入框（限制为数字）
- 显示单位 "毫秒 (ms)"
- 自动加载当前配置值

#### ✅ 波动范围输入框
- 带标签 "波动范围 (Variance)："
- 数字输入框（限制为数字）
- 显示单位 "毫秒 (ms)"
- 自动加载当前配置值

#### ✅ 开机自启动复选框
- 标签 "开机自启动"
- **关键实现**：读取实际的注册表状态来设置初始值
- 只在用户手动勾选/取消并点击保存时才修改注册表

#### ✅ 管理员权限部分
- 显示当前管理员权限状态文本
  - "当前状态：已以管理员权限运行" 或
  - "当前状态：普通权限运行"
- "请求管理员权限"按钮

#### ✅ 保存和取消按钮
- "保存"按钮：验证输入并保存配置
- "取消"按钮：关闭窗口不保存

### 2. 交互逻辑

#### ✅ 延迟模式切换自动更新
```ahk
OnDelayModeChange(dropdown, baseDelayEdit, varianceEdit)
```
- 当用户选择不同的延迟模式时
- 自动更新基础延迟和波动范围输入框
- 使用 `Delay_Algorithm.GetPresetValues()` 获取预设值

#### ✅ 输入验证
```ahk
ValidateInput(baseDelay, variance)
```
- 基础延迟：1-1000 毫秒
- 波动范围：0-500 毫秒
- 必须为整数
- 验证失败时显示错误消息并阻止保存

#### ✅ 保存设置逻辑
```ahk
OnSaveSettingsClick(settingsGui, delayModeDropdown, baseDelayEdit, varianceEdit, startupCheckbox)
```
- 验证所有输入
- 保存延迟模式、基础延迟、波动范围到 Config_Manager
- **关键实现**：智能处理开机自启动
  - 读取当前注册表状态
  - 只在状态改变时修改注册表
  - 同步更新配置文件
- 保存成功后更新托盘提示
- 显示保存成功提示并关闭窗口

#### ✅ 请求管理员权限
```ahk
OnRequestAdminClick()
```
- 调用 `Startup_Manager.RequestAdminElevation()`
- 如果已是管理员权限，显示提示
- 否则请求 UAC 提升并重启脚本

### 3. 关键特性

#### 🎯 开机自启动的正确实现
根据 CRITICAL REQUIREMENT，实现了以下逻辑：

1. **初始化时**：
   ```ahk
   actualStartupState := this.startupManager.IsStartupEnabled()
   startupCheckbox.Value := actualStartupState ? 1 : 0
   ```
   - 读取实际的注册表状态
   - 复选框反映真实状态

2. **保存时**：
   ```ahk
   currentStartupState := this.startupManager.IsStartupEnabled()
   
   if (startupEnabled && !currentStartupState) {
       ; 用户勾选了自启动，但当前未启用 -> 启用自启动
       this.startupManager.EnableStartup(A_ScriptFullPath)
       this.configManager.SetStartupEnabled(1)
   } else if (!startupEnabled && currentStartupState) {
       ; 用户取消了自启动，但当前已启用 -> 禁用自启动
       this.startupManager.DisableStartup()
       this.configManager.SetStartupEnabled(0)
   } else {
       ; 状态未改变，只更新配置文件中的值以保持一致
       this.configManager.SetStartupEnabled(startupEnabled ? 1 : 0)
   }
   ```
   - 只在用户手动改变状态并点击保存时才修改注册表
   - 避免不必要的注册表操作

## 测试方法

### 手动测试步骤

1. **运行测试脚本**：
   ```powershell
   # 运行独立测试脚本
   .\test_settings_window.ahk
   ```

2. **测试延迟模式切换**：
   - 选择 "Fast"，验证基础延迟变为 10，波动范围变为 5
   - 选择 "Standard"，验证基础延迟变为 30，波动范围变为 15
   - 选择 "Conservative"，验证基础延迟变为 50，波动范围变为 25

3. **测试输入验证**：
   - 输入基础延迟 0，点击保存，应显示错误消息
   - 输入基础延迟 1001，点击保存，应显示错误消息
   - 输入波动范围 -1，点击保存，应显示错误消息
   - 输入波动范围 501，点击保存，应显示错误消息

4. **测试开机自启动**：
   - 勾选"开机自启动"，点击保存
   - 验证注册表中是否创建了启动项
   - 重新打开设置窗口，验证复选框是否仍然勾选
   - 取消勾选，点击保存
   - 验证注册表中的启动项是否被删除

5. **测试管理员权限**：
   - 点击"请求管理员权限"按钮
   - 如果当前不是管理员，应弹出 UAC 提示
   - 如果当前已是管理员，应显示提示消息

6. **测试保存和取消**：
   - 修改设置，点击"取消"，验证设置未保存
   - 修改设置，点击"保存"，验证设置已保存到 config.ini

### 集成测试

运行完整的主脚本：
```powershell
.\you-can-copy-and-paste-v2.ahk
```

1. 右键点击托盘图标
2. 选择"设置"菜单项
3. 验证设置窗口正确显示
4. 测试所有功能

## 验证清单

- [x] 创建 GUI 窗口，设置标题和尺寸
- [x] 添加延迟模式下拉菜单（Fast/Standard/Conservative）
- [x] 添加基础延迟输入框（带标签和单位）
- [x] 添加波动范围输入框（带标签和单位）
- [x] 添加"开机自启动"复选框
- [x] 复选框反映实际注册表状态
- [x] 只在用户手动改变并保存时修改注册表
- [x] 添加"请求管理员权限"按钮
- [x] 显示当前管理员权限状态
- [x] 添加"保存"和"取消"按钮
- [x] 延迟模式切换时自动更新参数字段
- [x] 输入验证（1-1000ms 和 0-500ms）
- [x] 保存成功后更新托盘提示
- [x] 保存成功后显示提示消息

## 代码位置

- **主实现**：`you-can-copy-and-paste-v2.ahk` 中的 `UI_Manager` 类
  - `ShowSettingsWindow()` 方法（第 687-753 行）
  - `OnDelayModeChange()` 方法（第 755-765 行）
  - `OnRequestAdminClick()` 方法（第 767-771 行）
  - `OnSaveSettingsClick()` 方法（第 773-820 行）
  - `ValidateInput()` 方法（第 822-843 行）

- **测试脚本**：`test_settings_window.ahk`

## 符合要求

✅ 所有子任务已完成：
- ✅ 实现 ShowSettingsWindow() 方法
- ✅ 创建 GUI 窗口，设置标题和尺寸
- ✅ 添加延迟模式下拉菜单（Fast/Standard/Conservative）
- ✅ 添加基础延迟输入框（带标签和单位）
- ✅ 添加波动范围输入框（带标签和单位）
- ✅ 添加"开机自启动"复选框
- ✅ 添加"请求管理员权限"按钮
- ✅ 显示当前管理员权限状态
- ✅ 添加"保存"和"取消"按钮

✅ CRITICAL REQUIREMENT 已满足：
- 复选框反映实际注册表状态
- 只在用户手动勾选/取消并点击保存时才启用/禁用自启动

## 下一步

Task 8.3 已完成。可以继续执行 Task 8.4（实现设置界面交互逻辑）或进行集成测试。
