# Xbox popup fix

[中文](README.md) | English

Tested on **Windows 11 25H2**.

Once Xbox Game Bar has been uninstalled, pressing the Xbox button on a controller brings up *"You'll need a new app to open this ms-gamebar link"* every time. Importing the registry file in this repo makes it stop.

> Seeing a similar popup when you click links after removing Microsoft Edge? That is a separate problem — see [edge-removal-link-fix](https://github.com/AINLZE/edge-removal-link-fix).

## Fix with the file

1. Download `fix-gamebar-popup.reg`.
2. Double-click it and confirm the import. No administrator rights needed.

To revert, import `undo-gamebar-popup.reg`.

## What it changes

- Turns off `UseNexusForGameBarEnabled`, so the Xbox button no longer tries to open Game Bar.
- Routes `ms-gamebar` and `ms-gamebarservices` links to `systray.exe`, which exits immediately, so no window ever appears.

## Fix it by hand

Open `regedit`:

1. Under `HKEY_CURRENT_USER\Software\Microsoft\GameBar`, set the DWORD `UseNexusForGameBarEnabled` to `0`.
2. Create `HKEY_CURRENT_USER\Software\Classes\ms-gamebar` and add an empty string value named `URL Protocol`.
3. Under it, create `shell\open\command` and set `(Default)` to `"C:\Windows\System32\systray.exe"`.
4. Repeat steps 2 and 3 for `ms-gamebarservices`.

> This edits the registry. Use at your own risk.
