# Yison-AI Server — 賈維斯動態記憶
> 由 sync_claude_memory.sh 自動產生，每日 03:05 更新
> 最後更新：2026-04-11 11:51 CST
> 任務總數（14天）：102 條｜未完成：0 條｜重要記憶：35 條
> ⚠️ 此檔案不含任何密碼、Token 或機敏 IP

---

## 今日摘要（Daily Debrief）

賈維斯今日完成 `apt` 自動更新功能實作，並將記憶同步機制從固定排程改為閒置觸發。修復了記憶庫的 Git token 過期問題，並釐清網路偶發錯誤屬正常。重要決定包括簡化系統更新流程、採納按需記憶同步設計，同時持續優化 Cloudflare 快取設定，確保記憶即時性。


---

## 近兩週任務記錄

### 📅 2026-04-11 [general] 這是 **ShellFrame**，一個輕量級的多標籤 GUI 終端機包裝工具。
這是 **ShellFrame**，一個輕量級的多標籤 GUI 終端機包裝工具。 **專門為 Claude Code 設計的亮點：** • 可以直接 Cmd+V / Ctrl+V 貼截圖進終端，自動存成檔案並把路徑注入 Claude Code • 多標籤同時跑多個 AI 工作階段 • 可儲存預設指令（例如 `claude --dangerously-skip-permissions` 這種長指令...

### 📅 2026-04-11 [feature] MCP 整理 + /jarvis/run webhook 新增
完成兩大任務： 【任務一】MCP 整理 - 建立 Utils/Mcp/ 目錄結構：Jarvis-Memory/ / Web-Search/ / Docker/ / Archive/ - Operations/Health/jarvis-memory-mcp.py → Utils/Mcp/Jarvis-Memory/Server.py（複製） - Bots/Assistant/skills/mcp...

### 📅 2026-04-11 [config] 找到了！monitor.yison.io
找到了！monitor.yison.io 設計文件裡有規劃這個網域，路徑是 infra/monitoring/，但目前： • 資料夾不存在 • 網站沒有回應（HTTP 000） • nginx 也沒有 monitor.yison.io 的設定 應該是之前規劃過但還沒有建出來。 你想建立賈維斯專屬的監控 Dashboard 嗎？

### 📅 2026-04-11 [config] 對，如果開 AI 模式的話很吃。
對，如果開 AI 模式的話很吃。 但它有 `--ai none` 純提取模式，完全不用 token，直接用規則解析文件結構。 如果文件是原生文字 PDF（不是掃描圖），純提取模式效果就很好，不需要 AI 介入。 掃描圖才會需要 AI（OCR + 理解），那才真的燒 token。 所以看你的使用場景： • 原生 PDF / DOCX → 免 token • 掃描圖 PDF → 才需要 AI，...

### 📅 2026-04-11 [feature] 天氣預報修復 + GPS 位置 Webhook + Morning Greeting 更新
今日完成項目： 1. 修復天氣預報邏輯：    - 圖片城市（週末全球輪播）與天氣城市（使用者所在地）正確分離，不再混用 2. 新增 webhook /location/update endpoint：    - 接收 Tasker 傳來的 GPS 座標，存進 DB 3. 更新 morning_greeting.py：    - 天氣資訊改從 DB 讀取位置，預設城市為台北士林 4. 更新...

### 📅 2026-04-11 [infra] 測試 uptime
指令：uptime 結果： 11:50:46 up 20 days,  8:12,  3 users,  load average: 0.32, 0.26, 0.20

### 📅 2026-04-11 [general] 系統目前所有對外網站：
系統目前所有對外網站： 🖼 AI 相冊 https://gallery.yison.io Flask + Gunicorn，Google Drive 照片 + Gemini AI 分析 💰 薪資儀表板 https://salary.yison.io 華電聯網薪資收入管理（月/季統計） 📈 自選股看板 https://stock.yison.io 個人持股追蹤，即時股價（Yahoo Finan...

### 📅 2026-04-11 [config] 找到了！你說的應該是 **Uptime Kuma**（自架監控）。
找到了！你說的應該是 **Uptime Kuma**（自架監控）。 有設定檔（docker-compose），但目前**沒有在跑**（docker ps 沒看到 uptime-kuma 容器，port 3001 也沒回應）。 應該是之前規劃過但沒有正式部署，或者曾經跑過後被移除了。 要重新部署起來嗎？

### 📅 2026-04-11 [infra] 完成！健康檢查已更新為 7 個網站：
✅ 完成！健康檢查已更新為 7 個網站： 🌐 網站狀態：   ✅ gallery.yison.io: online   ✅ salary.yison.io: online   ✅ stock.yison.io: online   ✅ webhook.yison.io: online   ✅ x-twitter.yison.io: online   ✅ vault.yison.io: online...

### 📅 2026-04-11 [feature] 這是 **doc-cleaner**，一個文件清洗工具。
這是 **doc-cleaner**，一個文件清洗工具。 **主要功能：** 把 PDF、DOCX、XLSX、PPTX、TXT 等各種格式轉成乾淨的 Markdown **亮點：** • 專為繁體中文金融文件設計（對帳單、報表等） • 支援掃描 PDF、加密 PDF、格式破碎 PDF • 自動識別 Big5 / CP950 / UTF-16 編碼 • 可自動清除廣告、法律聲明等雜訊 • AI ...

### 📅 2026-04-11 [feature] 今天你丟來的三個，老實說：
今天你丟來的三個，老實說： ❌ Hermes Agent — 不需要，你的架構更完整 ❌ ShellFrame — 不需要，你用 Telegram 遠端操作 ⚠️ doc-cleaner — 有潛力，但看你有沒有文件處理需求 目前沒有看到「一定要裝」的工具。 主要是你的系統架構已經很完整了，大部分開源工具解決的問題你已經自己解決了。 如果要找真正有用的，我覺得比較值得關注的方向是： • *...

### 📅 2026-04-11 [feature] 這是**太空船大氣層跳躍再入（Skip Reentry）**示意圖，說明太空艙從月球返回地球的飛行過程：
這是**太空船大氣層跳躍再入（Skip Reentry）**示意圖，說明太空艙從月球返回地球的飛行過程： **6 個步驟：** 1. **從月球出發** — 速度約 40,000 km/h 2. **初次大氣層進入** — 接觸大氣層上緣，溫度約 2800°C 3. **跳躍（The Skip）** — 像打水漂一樣，太空艙不直接衝入大氣，而是「彈」回太空 4. **跳躍飛行階段（真空中）**...

### 📅 2026-04-11 [config] 已記錄（我就是賈維斯，直接存進去了）
✅ 已記錄（我就是賈維斯，直接存進去了） Memory id=248，今日 5 個重點全部寫入： • 天氣預報圖片城市 / 天氣城市分離 • /location/update webhook + GPS → DB • morning_greeting.py 從 DB 讀位置 • weather_report.py 加 photo_city 參數 • Daily Debrief 23:30 確認正...

### 📅 2026-04-11 [feature] 全部完成
✅ 全部完成 **1. Jarvis_Memory_Writer.py 修改：** importance=5 現在需要同時滿足： • 架構關鍵字（schema / 系統架構 / 重大決策...） • 系統相關（賈維斯 / yison / claude_agent / stock.yison...） 兩個條件缺一不可 **2. DB 清理：** 刪除 2 筆垃圾永久記憶 剩餘 importanc...

### 📅 2026-04-11 [infra] 這是 Nous Research 開發的開源 AI Agent 框架，叫 **Hermes Agent**。
這是 Nous Research 開發的開源 AI Agent 框架，叫 **Hermes Agent**。 ⭐ GitHub 51.9k 星，MIT 授權 **核心特色：** • 自我改進：會從對話經驗自動建立技能，持續學習 • 多平台：CLI / Telegram / Discord / Slack / WhatsApp / Signal 都能跑 • 支援 200+ 模型：OpenAI、A...

### 📅 2026-04-11 [infra] 不需要。原因如下：
不需要。原因如下： **你已經有的：** • 賈維斯 = Claude Code + Telegram（比它更強的模型） • 記憶系統 = Jarvis_Memory_Writer + PostgreSQL（客製化、完全掌控） • 排程 = crontab + PM2（夠用） • 多服務架構 = 自己的 nginx / systemd / Docker **Hermes 能給你什麼你沒有的：*...

### 📅 2026-04-11 [general] 對不起漏掉一個！完整清單：
對不起漏掉一個！完整清單： 🖼 AI 相冊 https://gallery.yison.io 💰 薪資儀表板 https://salary.yison.io 📈 自選股看板 https://stock.yison.io 📊 Usage 儀表板 https://usage.yison.io （靜態 HTML，輸出在 Web-Services/Usage-Dashboard/output/） ...

### 📅 2026-04-11 [general] usage.yison.io 是 Claude Code 用量報表
usage.yison.io 是 Claude Code 用量報表 追蹤 Claude Code 的 token 使用量、費用等，靜態 HTML 頁面，由腳本定期生成後放到 output/ 目錄。

### 📅 2026-04-11 [config] 健康檢查新增 vault.yison.io / usage.yison.io 監控
新增兩個網站到健康檢查監控清單： - vault.yison.io（Vaultwarden 密碼管理器） - usage.yison.io（Claude Code 用量報表） 修改檔案： 1. Operations/Health/System_Health.py：WEBSITES 清單新增兩筆 2. Operations/Health/Telegram_Repair_Trigger.py：WEB...

### 📅 2026-04-10 [infra] sync 完成
✅ sync 完成 MEMORY_PUBLIC.md 7710 bytes（12:53 CST） 已 push GitHub + 部署 webhook 現在可以測試 fetch https://webhook.yison.io/memory.md 確認是否拿到最新版本（最後更新：2026-04-10 12:53 CST）


---

## 未完成事項

（目前無未完成事項）

---

## 重要長期記憶

### ⭐5 全部完成
✅ 全部完成 **1. Jarvis_Memory_Writer.py 修改：** importance=5 現在需要同時滿足： • 架構關鍵字（schema / 系統架構 / 重大決策...） • 系統相關（賈維斯 / yison / claude_agent / stock.yison...） 兩個條件缺一不可 **2. DB 清理：** 刪除 2 筆垃圾永久記憶 剩餘 importanc...

### ⭐5 claude_agent DB 缺少 chat_logs 表
症狀：Daily Log Summary 報告 `relation "chat_logs" does not exist` 錯誤。根本原因：`yison-postgres-agent` 容器建立時只有 memories 表，chat_logs 表從未建立。解法：手動執行 CREATE TABLE：`docker exec yison-postgres-agent psql -U agent_adm...

### ⭐5 watchlist_alert_log 資料表
所屬 DB：stock_dashboard（stock_postgres, port 5433） 用途：自選股漲跌警示去重記錄 去重機制：ON CONFLICT DO NOTHING，每天每種警示只通知一次

### ⭐5 週選股資料表結構
由 weekly_picks.py 寫入週選股資料表，欄位包含：week_start、symbol、name、sector、direction、target_price、predict_pct、confidence、reasoning、base_price、ai_source 等

### ⭐5 watchlist_alert_log資料表
資料表：`watchlist_alert_log`，用途：記錄已發送的股票警示，防止同一天重複通知

### ⭐5 claude_agent importance等級
claude_agent importance 等級：1（一般）→ 5（DB schema/架構，永久保留）。等級 5 的資料永久保留不會被清理。

### ⭐5 memories資料表欄位
memories 資料表欄位包含：title、content、importance、expires_at、created_at。用於儲存重要記憶資料。

### ⭐5 task_logs資料表欄位
task_logs 資料表欄位包含：task_title、task_summary、category、importance、status、files_modified、created_at、expires_at。status 值為 completed 或 pending。

### ⭐5 claude_agent主要資料表
claude_agent 資料庫主要資料表： 1. memories：長期記憶，含 expires_at（NULL=永久），依 importance 設定到期時間 2. task_logs：任務記錄，含 status（pending/done）、importance、created_at

### ⭐5 weekly_picks表JSON欄位結構
weekly_picks 資料表 JSON schema 欄位： - rank：排名（1–5） - symbol：股票代號（純數字，如 2303） - name：股票名稱 - sector：類股分類 - direction：up 或 down - target_price：一週後預測股價 - predict_pct：預測漲跌幅（負數為跌） - reasoning：選股理由（繁體中文，50 字內）


---

## 服務現況快照

### systemd 服務

- ai-gallery: active
- salary-dashboard: active
- stock-dashboard: active

### PM2 服務
- telegram-bot: online
- mutual-watchdog: online
- webhook-service: online

### Docker 容器
- relaxed_carson: Up 10 hours
- stock_postgres: Up 2 days (healthy)
- vaultwarden: Up 2 days (healthy)
- yison-mcp-postgres: Up 2 days
- yison-postgres-agent: Up 2 days (healthy)
- yison-mcp-google-maps: Up 2 days
- cloudflare_tunnel: Up 2 days
- gallery_postgres: Up 2 days (healthy)
- pgadmin: Up 2 days

---

## 使用說明

將此連結的內容貼給 Claude AI 作為上下文記憶（即時版本，無 CDN cache）：
`https://webhook.yison.io/memory.md`
