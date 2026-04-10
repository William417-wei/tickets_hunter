# tickets_hunter 專案背景

這是一個合法的搶票機器人，支援多個售票平台（拓元 tixcraft、KKTIX 等）。

## Tixcraft 自動抓取 Cookie 功能

### 功能說明
settings 頁面新增「自動抓取」按鈕，讓使用者可以一鍵從本機 Chrome 讀取 TIXUISID cookie 並填入 `tixcraft_sid` 欄位。**不要在搶票過程中自動執行此動作。**

### 相關檔案
- `src/settings.py` — 後端，`FetchTixcraftCookieHandler` 處理 `/fetch_cookie` 路由
- `src/www/settings.html` — 前端，`tixcraft_sid` 欄位旁的「自動抓取」按鈕
- `src/www/settings.js` — 按鈕 click handler，呼叫 `/fetch_cookie` API

### Chrome Cookie 路徑（跨平台）
- **macOS**：`~/Library/Application Support/Google/Chrome`
- **Windows**：`%LOCALAPPDATA%\Google\Chrome\User Data`
- 兩個平台的 profile 子目錄結構相同（`Default`、`Profile 1` 等）

### 依賴套件
```
pip install browser-cookie3
```

### 注意事項
- JS 版本號在 `settings.html` 裡以 `settings.js?v=XXXXXXXXXX` 格式控制快取，每次修改 `settings.js` 後要更新版本號（目前是 `2026041001`）
- 按鈕的顯示由 `data-under="tixcraft"` 控制，只有在 homepage 設定為 tixcraft 網址時才會顯示
- `browser_cookie3` 會讀取 Chrome 的 SQLite cookie 資料庫並解密（macOS 用 Keychain，Windows 用 DPAPI）

### Web server
- settings 頁面跑在 port `16888`，執行：`python settings.py`
