# 🎮 GamerGuide - 會員制遊戲攻略發布系統 (CRUD & Auth)

[![Firebase](https://img.shields.io/badge/Backend-Firebase-FFCA28?style=flat&logo=firebase)](https://firebase.google.com/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**GamerGuide** 是一個整合了完整後端雲端服務的玩家創作區平台。本系統透過 **Firebase Authentication** 實作安全的 Google 一鍵登入機制，並結合 **Cloud Firestore** 達成全自動、零延遲的攻略文章 **CRUD (新增、讀取、更新、刪除)** 互動社群。

---

## ✨ 核心系統功能 (Core Features)

* **🔐 Google 帳號身分驗證 (Firebase Auth)：**
  * 整合 Google 生態系，支援玩家一鍵登入/登出。
  * 登入後自動安全擷取玩家的 Google 大頭貼 (`photoURL`) 與公開暱稱 (`displayName`)。
  * 智慧型介面控制：未登入時顯示鎖定提示，登入後自動解鎖專屬的「撰寫表單」。

* **動態動態 CRUD 系統 (Cloud Firestore)：**
  * **Create (發表)：** 自動綁定當前登入使用者的 `uid` 與伺服器時間，杜絕匿名冒名發文。
  * **Read (即時監聽)：** 採用 `onSnapshot` 建立單一即時資料串流。免重整網頁，全網玩家發文秒級同步刷新。
  * **Update & Delete (權限編輯與刪除)：** * 系統前端會自動比對當前登入者的 `uid` 與文章擁有者的 `userId`。
    * **只有作者本人**的卡片才會浮現「編輯」與「刪除」按鈕。
    * 提供「平滑滾動 (Smooth Scroll)」編輯置頂導引，以及防呆刪除確認彈窗。

---

## 🛠️ 技術棧 (Tech Stack)

| 技術區塊 | 項目 | 用途說明 |
| :--- | :--- | :--- |
| **前端骨架** | HTML5 / CSS3 | 語意化暗黑系視覺排版、動態表單樣式、響應式按鈕組設計 |
| **核心邏輯** | JavaScript (ES6+ Module) | DOM 操作、異步事件處理 (`async/await`)、狀態切換控制 |
| **身分驗證** | Firebase Auth | 提供 GoogleAuthProvider 彈窗登入與 `onAuthStateChanged` 狀態監聽 |
| **資料儲存** | Cloud Firestore | 資料結構化儲存 (`guides` 集合)、多重條件查詢與即時動態快照 |

---

## 🗄️ 後端資料欄位結構 (Firestore Data Schema)

在 Firestore 資料庫中，所有攻略皆安全儲存於 `guides` 集合 (Collection) 中，其單一文件 (Document) 的欄位結構設計如下：

```json
{
  "gameName": "遊戲名稱 (String)",
  "title": "攻略標題 (String)",
  "content": "詳細密技內文 (String)",
  "userId": "作者的 Firebase Auth UID (String)",
  "author": "作者 Google 帳號暱稱 (String)",
  "timestamp": "伺服器建立時間 (Timestamp)"
}
