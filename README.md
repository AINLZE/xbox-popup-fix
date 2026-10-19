# Xbox 弹窗修复

中文 | [English](README.en.md)

测试环境：**Windows 11 25H2**

卸载 Xbox Game Bar 后，每次按手柄上的 Xbox 键，都会弹出“你需要使用新应用打开此 ms-gamebar 链接”。导入本仓库的注册表文件即可让它不再出现。

> 卸载 Microsoft Edge 后点击链接也弹出类似窗口？那是另一个问题，见 [edge-removal-link-fix](https://github.com/AINLZE/edge-removal-link-fix)。

## 用文件修复

1. 下载 `fix-gamebar-popup.reg`。
2. 双击并确认导入，不需要管理员权限。

想恢复原状，导入 `undo-gamebar-popup.reg` 即可。

## 修复做了什么

- 关闭 `UseNexusForGameBarEnabled`，按 Xbox 键时不再尝试打开 Game Bar。
- 把 `ms-gamebar` 和 `ms-gamebarservices` 链接交给 `systray.exe` 处理。它启动后会立即退出，所以不会出现任何窗口。

## 不下载，手动修改

打开 `regedit`（注册表编辑器）：

1. 在 `HKEY_CURRENT_USER\Software\Microsoft\GameBar` 下，把 DWORD 值 `UseNexusForGameBarEnabled` 设为 `0`。
2. 新建 `HKEY_CURRENT_USER\Software\Classes\ms-gamebar`，添加一个空的字符串值 `URL Protocol`。
3. 在其下新建 `shell\open\command`，把 `(默认)` 设为 `"C:\Windows\System32\systray.exe"`。
4. 对 `ms-gamebarservices` 重复第 2、3 步。

> 本修复会修改注册表，请自行承担风险。
