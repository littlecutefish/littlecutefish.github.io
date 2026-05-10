---
title: "💼 旅途手帖 — 多人共遊旅遊規劃平台"
date: 2026-05-10 00:00:00 +0800
categories: [project]
tags: [code, tech, firebase, PWA]
---

## 專案簡介

因為我自己想要去日本玩，有抽空的時間就來創建一下自己的旅遊規劃平台。

**旅途手帖（Trip Planner）** 是一款以莫蘭迪色系設計、支援多人共同編輯的 PWA 旅遊規劃平台。

從行程建立、景點安排、多人分帳，到地圖瀏覽與旅途相簿，整合了旅行前中後所需的功能。本專案使用 **Claude Code** 輔助開發，以 Firebase 作為後端，無需自架伺服器。

> 🔗 **Live Demo：**[旅途手帖 Trip Planner](https://littlecutefish.github.io/trip/index.html)

---

## 功能總覽

| 頁面 | 功能 |
|------|------|
| **首頁** | 行程卡片總覽、建立 / 刪除行程、即時匯率顯示 |
| **行程規劃** | 天數管理、景點新增與編輯、景點複製/移動、每日備忘、注意事項、攜帶物品清單 |
| **記帳** | 多人分帳、幣別換算、圖表統計 |
| **地圖** | Leaflet 互動地圖、景點大頭針標示、底部抽屜行程列表 |
| **相簿** | 旅途照片上傳與瀏覽 |
| **好友** | 旅伴管理、邀請連結加入行程 |
| **個人資料** | 頭貼裁切、邊框顏色自訂、自我介紹 |

---

## 技術架構

### 前端
- 純 HTML / CSS / JavaScript（無框架）
- 莫蘭迪設計系統（CSS 變數統一管理色彩與元件）
- PWA：支援離線快取（Service Worker）、可安裝至桌面

### 後端 / 雲端服務
- **Firebase Authentication**：Google 一鍵登入
- **Firestore**：即時多人同步資料（行程、景點、記帳）
- **Firebase Storage**：相簿照片儲存
- **Firestore Security Rules**：依行程成員身分控制讀寫權限

### 第三方服務
- **Leaflet.js**：開源地圖套件
- 匯率 API：即時顯示日圓 / 台幣匯率

---

## 專案結構

```
trip/
├── index.html               ← 首頁（登入 + 行程列表）
├── sw.js                    ← Service Worker（PWA 快取）
├── firestore.rules          ← Firestore 安全規則
│
├── pages/
│   ├── trips.html           ← 行程列表
│   ├── itinerary.html       ← 行程規劃（天數 / 景點）
│   ├── expenses.html        ← 記帳
│   ├── map.html             ← 地圖
│   ├── memories.html        ← 相簿
│   ├── friends.html         ← 好友 / 旅伴
│   ├── profile.html         ← 個人資料
│   └── join.html            ← 邀請加入行程
│
├── js/
│   ├── app.js               ← 核心資料層
│   ├── auth.js              ← 登入 / 登出
│   ├── firebase-sync.js     ← Firestore 同步
│   ├── trips.js             ← 行程 CRUD、邀請
│   ├── itinerary.js         ← 天數 / 景點管理
│   ├── expenses.js          ← 記帳、分帳計算
│   ├── map-page.js          ← 地圖邏輯
│   ├── memories.js          ← 相簿
│   ├── friends.js           ← 好友列表
│   ├── profile.js           ← 個人資料
│   ├── join.js              ← 邀請連結處理
│   └── exchange.js          ← 匯率查詢
│
└── css/
    ├── style.css            ← 設計系統（莫蘭迪色系）
    ├── itinerary.css
    └── map-page.css
```

---

## 開發特色

- **Claude Code 輔助開發**：本專案主要透過 Claude Code CLI 工具進行功能設計與程式碼生成，大幅縮短開發週期。
- **多人即時同步**：透過 Firestore 即時監聽，多位旅伴同時編輯行程時可即時看到對方的變動。
- **邀請制協作**：透過專屬邀請連結加入行程，無需手動加好友。
- **PWA 支援**：可安裝至手機桌面，支援部分離線使用。

---

## 開發心得

這個專案其實是從一個很生活化的出發點開始的——因為想去日本玩，在規劃行程的過程中發現市面上的工具要嘛功能太雜，要嘛不支援多人協作，就乾脆自己做一個。

開發過程中大量使用了 **Claude Code** 來加速，從功能設計、資料結構規劃到 UI 細節調整都有它的參與。體驗下來確實感受到 AI 輔助開發的效率——很多過去要花時間查資料或反覆試錯的環節，現在可以在對話中快速解決，讓我可以把更多心思放在「這個功能到底好不好用」這件事上。

比較有挑戰的部分是多人同步的設計，要確保不同旅伴同時修改行程時不會互相蓋掉，Firestore 的即時監聽機制解決了大部分問題，但邊界情況還是需要自己去處理。

整體來說，這是一個從「自己有需求」出發、用來解決真實問題的小專案，也是我第一次認真把 PWA 和 Firebase 完整整合在一起。歡迎有興趣的朋友直接試用，或有想一起規劃旅遊的也可以找我 😄

p.s Bug還很多，所以同時也有在慢慢修正

---
