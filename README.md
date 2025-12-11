# 📰 Daily News Feed – 多來源新聞自動摘要推播（n8n Workflow）

此 workflow 建構於 **n8n**，可每天自動從多個 RSS 來源並行抓取新聞、解析 XML、依分類整理，再以 Markdown 形式推送至 Telegram。  
無需外部 RSS 套件，所有邏輯完全由 Code Node 控制，速度快且可自由擴充。

---

## ✨ 功能特色

### 🚀 並行抓取 RSS
- 使用 **HTTP Request Node** 一次處理所有來源
- 不受單一來源延遲拖累
- 支援 timeout 與錯誤不中斷

### 🧠 自動分類整理
- 所有來源由 Code Node 定義：`category / mySource / url`
- 每來源最多擷取 **5 則最新新聞**
- 自動清理標題中的特殊符號、避免 Markdown 格式破壞

### 📝 Telegram Markdown 排版
自動產生乾淨的訊息格式：

```
*💻 科技新知*

1. [標題](連結) - _iThome_
2. [另一則新聞](連結) - _TechNews_
```

每個分類會獨立推送一則訊息。

### ⏰ 每日定時推播
- 每天 **11:30** 自動發送
- 若要改時間可直接編輯 ScheduleTrigger 節點

---

## 🖼️ 截圖
[![Workflow 截圖](/workflow.png)](/workflow.png)
---
[![Telegram 截圖](/telegram.png)](/telegram.png)

---

## 🧱 Workflow 節點架構總覽

### 1️⃣ 每日排程
**每天早上11:30觸發**
- 啟動整個新聞推播流程

---

### 2️⃣ 定義新聞來源  
**設定新聞來源（Code Node）**
- 回傳一組來源列表，每筆包含：

```json
{
  "category": "💻 科技新知",
  "mySource": "TechNews",
  "url": "https://technews.tw/tn-rss/"
}
```

你可以自由新增或刪除任何來源。

---

### 3️⃣ 並行抓取 RSS  
**並行抓取 RSS（HTTP Request）**
- 對所有來源同時發起 HTTP GET
- 回傳原始 XML 字串供後續解析
- 問題來源不會中斷流程（continue on error）

---

### 4️⃣ 合併原始資料  
**合併原始資料（Merge）**
- 將來源資訊與 RSS 回應合併成單一資料流

---

### 5️⃣ 解析 XML + 整理成訊息  
**整理成 Telegram 訊息（Code Node）**

功能包含：

- 使用正則表達式解析 `<item>` 區塊
- 擷取：`title`、`link`、來源名稱
- 每來源最多 5 則
- 依 category 分組
- 組裝成乾淨的 Markdown 訊息

---

### 6️⃣ 推送到 Telegram  
**發送 Telegram**
- 將整理後的每個分類訊息分別推送到你的 Chat ID
- 使用 Markdown 模式呈現更好閱讀的格式

---

## 🚀 使用方式

### 1️⃣ 匯入 workflow JSON
匯入 `Daily News Feed.json`。

### 2️⃣ 設定 Telegram Chat ID  
在 **發送 Telegram** 節點修改：

```
chatId: <你的 Chat ID>
```

### 3️⃣ 新增 / 編輯新聞來源  
修改 **設定新聞來源** 節點內的陣列：

```js
{
  category: "🌏 國際財經",
  mySource: "中央社國際",
  url: "https://feeds.feedburner.com/rsscna/intworld"
}
```

---

## 📩 推播示例

```
🔥 頭條焦點

1. [重大新聞標題](https://example.com) - _聯合頭條_
2. [另一則新聞](https://example.com) - _中央社政治_
```

---

## 📬 聯絡作者
若你對 n8n、自動化、AI 整合有興趣，歡迎交流：

- GitHub：[@JoshuaWang2211](https://github.com/JoshuaWang2211)

---

本 workflow 旨在提供快速、乾淨、可客製化的每日新聞摘要推播，可作為 Email 電子報、多平台推播或個人化內容聚合的基礎。
