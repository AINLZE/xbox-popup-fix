# Windows 11 弹窗修复

[English](README.md) | 中文

测试环境：**Windows 11 25H2**

修复两种情况下的“你的设备需要一个新应用才能打开此链接”弹窗：

| 弹窗 | 出现时机 | 修复文件 |
|---|---|---|
| `ms-gamebar` | 卸载 Xbox Game Bar 后，按手柄上的 Xbox 键 | `fix-gamebar-popup.reg`（无需管理员） |
| `https` / `http` | 强制卸载 Microsoft Edge 后点击链接（卸载时连带删除了系统的 http/https 协议定义） | `fix-https-popup.reg`（需要管理员） |

## 用文件修复

下载对应的 `.reg` 文件，双击并确认导入。
撤销 Game Bar 修复：运行 `undo-gamebar-popup.reg`。

## 不下载，手动修改

打开 `regedit`（注册表编辑器）。

**ms-gamebar**

1. `HKEY_CURRENT_USER\Software\Microsoft\GameBar` → DWORD 值 `UseNexusForGameBarEnabled` 改为 `0`
2. 新建 `HKEY_CURRENT_USER\Software\Classes\ms-gamebar`，添加空字符串值 `URL Protocol`
3. 在其下新建 `shell\open\command`，`(默认)` 设为 `"C:\Windows\System32\systray.exe"`
4. 对 `ms-gamebarservices` 重复第 2–3 步

**https / http**

1. 新建 `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\https`，添加空字符串值 `URL Protocol`
2. 对 `http` 做同样的操作
3. 如果 `HKEY_CURRENT_USER\Software\Classes\https`（或 `http`）存在，也添加 `URL Protocol`

如果链接仍打不开，到“设置 › 应用 › 默认应用”里重新选一次浏览器。

> 会修改注册表，风险自负。
