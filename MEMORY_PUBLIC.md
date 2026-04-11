# Yison-AI Server — 賈維斯動態記憶
> 由 sync_claude_memory.sh 自動產生，每日 03:05 更新
> 最後更新：2026-04-11 23:30 CST
> 任務總數（14天）：106 條｜未完成：0 條｜重要記憶：41 條
> ⚠️ 此檔案不含任何密碼、Token 或機敏 IP

---

## 今日摘要（Daily Debrief）

賈維斯今日完成V3全面新設計，導入真實照片與AI評析。修復Claude.ai 401、FastMCP DNS問題及天氣預報邏輯等多項錯誤。建立了Yison-Bridge MCP Server；並決定Claude AI通知規則、評估外部工具（doc-cleaner具潛力），且將依據回饋調整V3設計方向。


---

## 近兩週任務記錄

### 📅 2026-04-11 [feature] 全部完成
✅ 全部完成 **1. Jarvis_Memory_Writer.py 修改：** importance=5 現在需要同時滿足： • 架構關鍵字（schema / 系統架構 / 重大決策...） • 系統相關（賈維斯 / yison / claude_agent / stock.yison...） 兩個條件缺一不可 **2. DB 清理：** 刪除 2 筆垃圾永久記憶 剩餘 importanc...

### 📅 2026-04-11 [feature] 修好了！
✅ 修好了！ **問題原因：** FastMCP 1.27.0 內建 DNS rebinding 保護，會拒絕 Host header 不是 localhost/127.0.0.1 的請求。 **修復方式：** 在 FastMCP 初始化加入 `allowed_hosts=["mcp.yison.io", "localhost", "127.0.0.1"]` **測試結果：** ``` # ...

### 📅 2026-04-11 [feature] 這是 **doc-cleaner**，一個文件清洗工具。
這是 **doc-cleaner**，一個文件清洗工具。 **主要功能：** 把 PDF、DOCX、XLSX、PPTX、TXT 等各種格式轉成乾淨的 Markdown **亮點：** • 專為繁體中文金融文件設計（對帳單、報表等） • 支援掃描 PDF、加密 PDF、格式破碎 PDF • 自動識別 Big5 / CP950 / UTF-16 編碼 • 可自動清除廣告、法律聲明等雜訊 • AI ...

### 📅 2026-04-11 [config] 已記錄（我就是賈維斯，直接存進去了）
✅ 已記錄（我就是賈維斯，直接存進去了） Memory id=248，今日 5 個重點全部寫入： • 天氣預報圖片城市 / 天氣城市分離 • /location/update webhook + GPS → DB • morning_greeting.py 從 DB 讀位置 • weather_report.py 加 photo_city 參數 • Daily Debrief 23:30 確認正...

### 📅 2026-04-11 [config] 286 行，內容正確 ✅
286 行，內容正確 ✅ ``` jarvis-memory-mcp.py — 賈維斯記憶 MCP Server 連接 claude_agent PostgreSQL DB 提供 6 個工具：   save_memory   search_memory   list_pending_tasks   complete_task   get_recent_tasks   get_important_...

### 📅 2026-04-11 [feature] Yison-Bridge MCP Server 完成！
✅ Yison-Bridge MCP Server 完成！ **🌐 MCP Server URL（待 Cloudflare 設定後）：** https://mcp.yison.io/sse **🔑 認證：** `Authorization: Bearer YISON_BRIDGE_2026` 或 `?token=YISON_BRIDGE_2026` **🛠 可用工具（7 個）：** • `s...

### 📅 2026-04-11 [config] 找到了！你說的應該是 **Uptime Kuma**（自架監控）。
找到了！你說的應該是 **Uptime Kuma**（自架監控）。 有設定檔（docker-compose），但目前**沒有在跑**（docker ps 沒看到 uptime-kuma 容器，port 3001 也沒回應）。 應該是之前規劃過但沒有正式部署，或者曾經跑過後被移除了。 要重新部署起來嗎？

### 📅 2026-04-11 [infra] 完成！健康檢查已更新為 7 個網站：
✅ 完成！健康檢查已更新為 7 個網站： 🌐 網站狀態：   ✅ gallery.yison.io: online   ✅ salary.yison.io: online   ✅ stock.yison.io: online   ✅ webhook.yison.io: online   ✅ x-twitter.yison.io: online   ✅ vault.yison.io: online...

### 📅 2026-04-11 [feature] 今天你丟來的三個，老實說：
今天你丟來的三個，老實說： ❌ Hermes Agent — 不需要，你的架構更完整 ❌ ShellFrame — 不需要，你用 Telegram 遠端操作 ⚠️ doc-cleaner — 有潛力，但看你有沒有文件處理需求 目前沒有看到「一定要裝」的工具。 主要是你的系統架構已經很完整了，大部分開源工具解決的問題你已經自己解決了。 如果要找真正有用的，我覺得比較值得關注的方向是： • *...

### 📅 2026-04-11 [feature] 天氣預報修復 + GPS 位置 Webhook + Morning Greeting 更新
今日完成項目： 1. 修復天氣預報邏輯：    - 圖片城市（週末全球輪播）與天氣城市（使用者所在地）正確分離，不再混用 2. 新增 webhook /location/update endpoint：    - 接收 Tasker 傳來的 GPS 座標，存進 DB 3. 更新 morning_greeting.py：    - 天氣資訊改從 DB 讀取位置，預設城市為台北士林 4. 更新...

### 📅 2026-04-11 [feature] MCP 整理 + /jarvis/run webhook 新增
完成兩大任務： 【任務一】MCP 整理 - 建立 Utils/Mcp/ 目錄結構：Jarvis-Memory/ / Web-Search/ / Docker/ / Archive/ - Operations/Health/jarvis-memory-mcp.py → Utils/Mcp/Jarvis-Memory/Server.py（複製） - Bots/Assistant/skills/mcp...

### 📅 2026-04-11 [feature] 全部完成！
✅ 全部完成！ 【任務一】MCP 整理 • Utils/Mcp/Jarvis-Memory/Server.py ✅ • Utils/Mcp/Web-Search/Server.py ✅ • Utils/Mcp/Docker/（3 個 Dockerfile）✅ • mcp_bridge.js → Utils/Mcp/Archive/ ✅ • 其他廢棄檔案 → Bots/Assistant/Arch...

### 📅 2026-04-11 [config] 以下是 Claude AI 問的 4 個問題的答案：
以下是 Claude AI 問的 4 個問題的答案： **Q1: claude.ai Remote MCP 認證方式** ~/.claude.json 裡的 MCP 設定只有 stdio 模式，沒有 OAuth/token 範例。 claude.ai 連 Remote MCP 用的是 **OAuth 2.0**，但自架最簡單的方式是： • URL 帶 token（`https://mcp.yis...

### 📅 2026-04-11 [feature] Yison-Bridge MCP Server 建立（Remote MCP for claude.ai）
建立了供 claude.ai 遠端連線的 MCP Server： 檔案位置：Utils/Mcp/Yison-Bridge/Server.py PM2 名稱：yison-mcp-bridge（id=5） Port：127.0.0.1:3031 對外網域：mcp.yison.io（需在 Cloudflare Dashboard 新增 Public Hostname） 認證：Bearer token...

### 📅 2026-04-11 [general] 有！V3 全面重設計完成 👆
有！V3 全面重設計完成 👆 這次改了： 📸 真實房屋照片（客廳/主臥/次臥/廚房/衛浴全部到位） 📐 格局圖專區（大圖顯示） 🤖 AI 智能評析（地段/交通/投資/生活 四項評分） 🎬 動畫：數字滾動 + 捲動進場 + 主圖自動輪播 ☀️ 亮色主題，清楚易讀 🔗 https://gallery.yison.io/Morning_PNG/edm_v3.html

### 📅 2026-04-11 [infra] 不需要。原因如下：
不需要。原因如下： **你已經有的：** • 賈維斯 = Claude Code + Telegram（比它更強的模型） • 記憶系統 = Jarvis_Memory_Writer + PostgreSQL（客製化、完全掌控） • 排程 = crontab + PM2（夠用） • 多服務架構 = 自己的 nginx / systemd / Docker **Hermes 能給你什麼你沒有的：*...

### 📅 2026-04-11 [config] 找到了！monitor.yison.io
找到了！monitor.yison.io 設計文件裡有規劃這個網域，路徑是 infra/monitoring/，但目前： • 資料夾不存在 • 網站沒有回應（HTTP 000） • nginx 也沒有 monitor.yison.io 的設定 應該是之前規劃過但還沒有建出來。 你想建立賈維斯專屬的監控 Dashboard 嗎？

### 📅 2026-04-11 [config] 健康檢查新增 vault.yison.io / usage.yison.io 監控
新增兩個網站到健康檢查監控清單： - vault.yison.io（Vaultwarden 密碼管理器） - usage.yison.io（Claude Code 用量報表） 修改檔案： 1. Operations/Health/System_Health.py：WEBSITES 清單新增兩筆 2. Operations/Health/Telegram_Repair_Trigger.py：WEB...

### 📅 2026-04-11 [config] 對，如果開 AI 模式的話很吃。
對，如果開 AI 模式的話很吃。 但它有 `--ai none` 純提取模式，完全不用 token，直接用規則解析文件結構。 如果文件是原生文字 PDF（不是掃描圖），純提取模式效果就很好，不需要 AI 介入。 掃描圖才會需要 AI（OCR + 理解），那才真的燒 token。 所以看你的使用場景： • 原生 PDF / DOCX → 免 token • 掃描圖 PDF → 才需要 AI，...

### 📅 2026-04-11 [bug_fix] 全部查完，問題已修好！
✅ 全部查完，問題已修好！ **Log 分析：** claude.ai 有連進來，/sse 成功（200）但 /messages/ 回 401 → 已修：現在 /messages/ 不需要 token（session_id 就是憑證） **FastMCP SSE 路徑：** • 連線用 `/sse` → `https://mcp.yison.io/sse` • 訊息傳遞用 `/messages...


---

## 未完成事項

（目前無未完成事項）

---

## 重要長期記憶

### ⭐5 Claude AI 使用 send_to_telegram 規則
send_to_telegram 只在兩種情況使用： 1. 賈維斯在背景執行任務完成，需要主動通知主人 2. 主人明確說「發給我的手機」 其他情況（笑話、查詢結果、一般回覆）直接在 claude.ai 對話裡回覆，不要發 Telegram。

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


---

## 服務現況快照

### systemd 服務

- ai-gallery: unknown
- salary-dashboard: unknown
- stock-dashboard: unknown

### PM2 服務
- telegram-bot: online
- mutual-watchdog: online
- webhook-service: online
- yison-mcp-bridge: online

### Docker 容器
- relaxed_carson: Up 22 hours
- stock_postgres: Up 3 days (healthy)
- vaultwarden: Up 3 days (healthy)
- yison-mcp-postgres: Up 3 days
- yison-postgres-agent: Up 3 days (healthy)
- yison-mcp-google-maps: Up 3 days
- cloudflare_tunnel: Up 3 days
- gallery_postgres: Up 3 days (healthy)
- pgadmin: Up 3 days

---

## 使用說明

將此連結的內容貼給 Claude AI 作為上下文記憶（即時版本，無 CDN cache）：
`https://webhook.yison.io/memory.md`
