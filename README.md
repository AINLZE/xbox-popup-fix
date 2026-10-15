# Windows 11 popup fixes

English | [中文](README.zh-CN.md)

Tested on **Windows 11 25H2**.

Fixes the *"You'll need a new app to open this link"* popup in two cases:

| Popup | When it appears | Fix |
|---|---|---|
| `ms-gamebar` | Pressing the Xbox button on a controller after Xbox Game Bar was removed | `fix-gamebar-popup.reg` (no admin) |
| `https` / `http` | Clicking links after Microsoft Edge was force-removed (the removal also deletes the system http/https protocol keys) | `fix-https-popup.reg` (admin) |

## Use the files

Download the `.reg` file, double-click it, and confirm.
To undo the Game Bar fix, run `undo-gamebar-popup.reg`.

## Or do it by hand

Open `regedit`.

**ms-gamebar**

1. `HKEY_CURRENT_USER\Software\Microsoft\GameBar` → DWORD `UseNexusForGameBarEnabled` = `0`
2. Create `HKEY_CURRENT_USER\Software\Classes\ms-gamebar`, add an empty string value `URL Protocol`
3. Under it create `shell\open\command`, set `(Default)` to `"C:\Windows\System32\systray.exe"`
4. Repeat steps 2–3 for `ms-gamebarservices`

**https / http**

1. Create `HKEY_LOCAL_MACHINE\SOFTWARE\Classes\https`, add an empty string value `URL Protocol`
2. Do the same for `http`
3. If `HKEY_CURRENT_USER\Software\Classes\https` (or `http`) exists, add `URL Protocol` there too

If links still don't open, pick your browser again in *Settings › Apps › Default apps*.

> Edits the registry. Use at your own risk.
