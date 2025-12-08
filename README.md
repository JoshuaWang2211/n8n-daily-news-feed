# 📰 Daily News Feed – 自動化多來源新聞摘要推播（n8n Workflow）

這是一個建構在 **n8n** 上的每日新聞摘要推播流程。會自動從多個 RSS 來源抓取最新新聞，依分類整理後，以 Markdown 格式推送到 Telegram。使用相同邏輯，也可以改為 html 格式，以 Email 電子報形式寄出。版面配置、新聞來源與種類都可以自訂。

---

## ✨ 功能特色

### 🔗 多來源 RSS 聚合
- 支援多組 RSS Feed
- 自動依分類整理（如頭條焦點、科技新知）
- 每個來源擷取最新 **5 則** 新聞

### 🧠 智慧分類與格式化
- 來源列表由 Code Node 定義（含 URL、分類、mySource）
- 自動合併所有來源的新聞
- 以乾淨易讀的 Markdown 推送

### 🤖 每日定時推播
- 每天早上 **11:30** 自動推播
- 支援 Telegram 推送

---

## 🖼️ 截圖
[![Workflow 截圖](/workflow.png)](/workflow.png)

---

[![Telegram 截圖](/telegram.png)](/telegram.png)

---

## 🧱 Workflow 節點架構總覽

### 1️⃣ 排程觸發
節點：**每天早上11:30點觸發**
- 每天固定時間啟動流程。

### 2️⃣ 定義新聞來源
節點：**設定新聞來源**
- 使用 Code Node 回傳一組 RSS 來源清單。
- 格式包含：`category`、`mySource`、`url`。

來源範例：
- 🔥 頭條焦點：聯合新聞、中央社
- 💻 科技新知：iThome、TechNews、Inside

### 3️⃣ 迴圈逐一讀取來源
節點：**迴圈處理 (SplitInBatches)**
- 每次處理一筆來源 URL。

### 4️⃣ 讀取 RSS
節點：**讀取 RSS**
- 自動抓取 RSS feed 資料。
- 錯誤不中斷（continue on error）。

### 5️⃣ 限制為最新 5 則
節點：**只取前5則**
- 避免產生過多內容。

### 6️⃣ 合併新聞資料
節點：**合併資料 (Multiplex)**
- 將所有來源的資料一次合併，供後續清理與格式化。

### 7️⃣ 整理成 Markdown 訊息
節點：**整理成訊息**
- 將所有資料依分類分組。
- 每組最多顯示 15 則。
- 轉換成 Telegram Markdown 格式：
  ```
  1. [標題](連結) - _來源_
  ```
- 自動排版、清理標題中不必要符號。

### 8️⃣ 推送到 Telegram
節點：**發送 Telegram**
- 每個分類各發送一則訊息。
- 支援 Markdown 樣式。

---

## 🚀 使用方式

### 1️⃣ 匯入 workflow JSON
將 `Daily News Feed.json` 匯入 n8n。

### 2️⃣ 設定 Telegram Chat ID
至節點 **發送 Telegram** 修改：
```
chatId: <你的 Chat ID>
```

### 3️⃣ 新增 / 編輯新聞來源
於節點 **設定新聞來源** 中修改：
```js
{
  json: {
    category: "💻 科技新知",
    mySource: "TechNews",
    url: "https://technews.tw/tn-rss/"
  }
}
```

---

## 📩 推播訊息示例
Telegram 會收到如下格式：

```
🔥 頭條焦點

1. [重大新聞標題](https://example.com) - _聯合頭條_
2. [另一則新聞](https://example.com) - _中央社_
```

---

## 📬 聯絡作者
若你對 n8n、AI、自動化整合有興趣，歡迎交流：

- GitHub：[@JoshuaWang2211](https://github.com/JoshuaWang2211)

---

本 workflow 由 Joshua Wang 製作，旨在自動化新聞摘要推播，適合每日追蹤新聞、整理資訊、或延伸至個人化內容聚合應用。

