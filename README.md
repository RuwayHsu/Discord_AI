# Discord\_AI機器人（全域唯一推理版）

本專案為個人使用設計的 Discord AI 機器人，基於 **Discord.py** 與 **本地大型語言模型（Qwen-1.5-4B-Chat）** 建置，搭配 **FAISS** 向量資料庫及記憶檢索機制，提供穩定、安全且資源高效的互動式問答服務。

---

## 專案簡介

此 AI 機器人專案的開發完全由 GPT-4.1 模型全程設計與撰寫，並由熊熊負責全程監督、運行、測試與 Debug，保證專案品質與效能。

核心設計強調單一用戶推理原則，確保所有伺服器與頻道中僅允許一人同時進行 AI 推理，避免主機資源耗盡，適合個人私有部署環境。

---

## 專案技術棧

* Discord.py（Discord 機器人框架）
* transformers 或 llama-cpp-python（本地大型語言模型推理）
* sentence-transformers（向量嵌入模型）
* FAISS（高效能向量檢索資料庫）
* Python 3.10+（推薦虛擬環境管理）

---

## 目錄結構

```
/
├── rag_system-Qwen1.5-4B-Chat/
│   ├── embedder.py          # 問答向量化索引生成
│   ├── retriever.py         # 記憶檢索
│   ├── prompt_builder.py    # Prompt 組建模組
│   ├── qwen_runner.py       # 本地模型推理執行
│   ├── main.py              # 命令列測試用主程式
│   ├── model/               # 存放模型檔
│   │   └── Qwen1.5-4B-Chat.gguf（或transformers格式）
│   ├── emb_model/           # embedding 模型
│   └── data/
│       ├── memory.json      # 問答記憶檔
│       ├── memory.index     # FAISS向量索引
│       └── summary_YYYYMMDD.json # 每日互動摘要
├── discord_bot.py           # Discord Bot主程式（單一使用版）
├── whitelist.json           # 白名單設定
├── sample.json              # Bot Token和管理伺服器設定
└── requirements.txt         # Python依賴套件
```

---

## 核心功能

### 1. 全域唯一推理鎖

* 同一時刻全系統僅允許一位用戶推理，避免資源爭搶。
* 其他用戶呼叫將獲得友善排隊提示。

### 2. 記憶與向量檢索

* 每次互動自動寫入 `memory.json`，並透過 `embedder.py` 轉為向量索引。
* 支援快速檢索歷史記憶，提升回覆相關性。

### 3. BigO 進度條

* 提供友善且視覺化的推理進度條，提升使用者體驗。

### 4. Discord 斜線指令管理

* `/help` 顯示幫助
* `/ping` 顯示延遲
* `/info` 顯示機器人資訊
* `/whitelist` 白名單設定（僅管理員）
* `/status` 機器人狀態查詢（僅管理員）
* `/clear_locks` 強制解鎖推理狀態（僅管理員）

---

## 部署與執行

### 安裝依賴套件

```bash
pip install -r requirements.txt
```

### 配置設定檔

* **`sample.json`**

```json
{
  "info": { "Token": "你的DiscordBotToken" },
  "Guild_ID": 你的管理伺服器ID
}
```

* **`whitelist.json`**

```json
{
  "guilds": { "伺服器ID": ["允許頻道ID"] }
}
```

### 啟動 Bot

```bash
python discord_bot.py
```

---

## 維護與建議

* 定期備份 `memory.json`、`memory.index`、`summary_*.json`。
* 於記憶更新後，執行一次 `embedder.py` 更新索引。
* 若遇到異常狀況，使用管理指令 `/clear_locks` 重置。

---

## 適用場景

* 個人專用 AI 助理
* 資源有限的本地部署環境
* 重視資料私密性與安全性

---

## 專案開發

本專案完全由 GPT-4.1 模型協助設計與開發，並由熊熊負責全程監督、運行、測試與 Debug，確保運作穩定與效能。

如有其他需求或回報問題，請隨時聯繫專案負責人（熊熊）。
