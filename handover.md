# 📦 LINE 訂位系統專案交接文件

## 🔰 專案簡介

本專案為 LINE LIFF + Google Sheet 建構的訂位系統，整合：

- LINE 登入（取得 UID、使用者名稱）
- Google 試算表作為後端資料庫
- Apps Script 做為伺服端邏輯（處理 doGet/doPost）
- GitHub Pages 作為前端網頁展示
- 已支援多功能：可預約時間載入、避免重複預約、成功頁面跳轉、資料寫入試算表等

## ✅ 已完成功能清單

### 📱 LINE 功能
- 取得使用者 UID、名稱、社群暱稱（自填欄位）
- 使用 LIFF 登入取得 LINE 使用者資訊

### 📅 訂位功能
- 從 Google Sheets 動態載入可預約時段（每日自動同步）
- 若該時段已被預約 → 前端會隱藏、不再顯示
- 支援毫秒級寫入避免重複預約（race condition）
- 成功預約會跳轉至 `success.html` 頁面
- 所有欄位均有灰底說明，並提供驗證提示

### 🔒 安全性強化
- 使用 ScriptProperties 儲存敏感參數（如 Spreadsheet ID 與工作表名稱）
- 不公開試算表
- Web App 授權為「由我執行」，「任何人可存取」

## 🧩 系統架構

```
├── index.html         # 前端主頁（LINE LIFF + 表單 + JS）
├── success.html       # 預約成功後顯示的跳轉頁面
├── script.js          # 前端邏輯（已整合 LIFF + 表單驗證）
└── Code.gs            # GAS 程式（doGet / doPost）
```

## 🚀 建置與部署流程

### 1️⃣ GitHub Pages
- 網址：https://coolka-hsu.github.io/line-booking-Wolf/
- 將 `index.html` 與 `success.html` 放於 repo 根目錄

### 2️⃣ Google Apps Script（GAS）
- 檔案名稱：`Code.gs`
- 授權方式：「由我執行」、「任何人可存取」
- ✅ Script Properties 設定：

| Key                  | Value                                                   |
|----------------------|----------------------------------------------------------|
| BOOKING_SHEET_ID     | 1fGhYzE97vjSR3Om1moKUGZqqzRFFxkchtht5jOHeNx8              |
| OPEN_SHEET_NAME      | 開放訂位時間                                             |
| BOOKING_SHEET_NAME   | 訂位資訊表                                               |

### 3️⃣ 試算表欄位規範

#### 🔓 工作表「開放訂位時間」
| 欄位 | 說明               |
|------|--------------------|
| A    | 用餐日期           |
| B    | 用餐時段           |
| C    | 是否已訂（"已訂"） |
| D    | 預約時間戳         |

#### 📝 工作表「訂位資訊表」
| 欄位 | 說明                     |
|------|--------------------------|
| A    | 訂單時間戳               |
| B    | UID                      |
| C    | 使用者名稱（LINE）      |
| D    | 訂位大名                 |
| E    | 用餐日期                 |
| F    | 用餐時段                 |
| G    | LINE 社群暱稱（輸入）   |
| H    | 電話（輸入）             |
| I    | 人數                     |
| J    | 備註（小孩年齡等）       |

## 🧪 前端驗證與 UX 補強

- 大人 / 小孩人數下拉選單（靜態 0~10）
- 總人數驗證限制為 10 人內
- 若所有時段皆已被預訂：顯示「本月訂位已全數額滿，請關注 LINE 社群」

## 🔮 可開發功能（未完成）

| 功能                 | 說明                             |
|----------------------|----------------------------------|
| LINE Notify/Broadcast | 傳送 LINE 群發通知（需 LINE 官方帳號） |
| 訂位後台查詢/取消    | UI 後台與取消功能需整合           |
| 月份選單             | 篩選「下個月」或更多可預約時段    |
| Email 通知           | 可用 Gmail API 發送提醒信         |
| 用餐前提醒通知       | GAS 時間觸發器自動提醒使用者       |

## 🙋 開發注意事項

- 所有敏感資訊皆使用 `ScriptProperties` 管理
- 前端請勿讀取 Google Sheet，避免 CORS 與權限問題
- Web App 每次部署請選擇「更新現有部署」保持 URL 不變

## 🧑‍💻 Maintainer

- 建立者：Senteurdoc / coolka-hsu  
- GitHub Repo: https://github.com/coolka-hsu/line-booking-Wolf  
- 試算表連結：https://docs.google.com/spreadsheets/d/1fGhYzE97vjSR3Om1moKUGZqqzRFFxkchtht5jOHeNx8