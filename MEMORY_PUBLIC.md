# Yison-AI Server — 賈維斯動態記憶
> 由 sync_claude_memory.sh 自動產生，每日 03:05 更新
> 最後更新：2026-04-11 03:00 CST
> 任務總數（14天）：83 條｜未完成：0 條｜重要記憶：30 條
> ⚠️ 此檔案不含任何密碼、Token 或機敏 IP

---

## 今日摘要（Daily Debrief）

賈維斯今日完成 `apt` 自動更新功能實作，並將記憶同步機制從固定排程改為閒置觸發。修復了記憶庫的 Git token 過期問題，並釐清網路偶發錯誤屬正常。重要決定包括簡化系統更新流程、採納按需記憶同步設計，同時持續優化 Cloudflare 快取設定，確保記憶即時性。


---

## 近兩週任務記錄

### 📅 2026-04-10 [feature] MCP Server 測試記憶
jarvis-memory-mcp.py 建立完成，5個工具正常運作

### 📅 2026-04-10 [config] [待辦] 確認諸葛亮自動回傳記憶連結功能
status=pending。明天早上打招呼時，確認諸葛亮是否能自動回傳記憶連結功能是否正常運作。屬於測試驗證類。

### 📅 2026-04-10 [config] [待辦] 確認 Daily Debrief 第一次自動執行
status=pending。今晚 23:30 確認 Daily Debrief 第一次自動執行，Gemini 摘要是否正確寫入 DB。屬於測試驗證類。

### 📅 2026-04-10 [config] [待辦] 確認 sync 自動更新 MEMORY_PUBLIC.md 並 push GitHub
status=pending。明天 03:05 確認 crontab sync 任務是否自動更新 MEMORY_PUBLIC.md 並 push 到 GitHub。屬於測試驗證類。

### 📅 2026-04-10 [config] [待辦] 批量匯入舊 Knowledge/Memory/*.md 重要內容到 DB
status=pending。把舊的 Knowledge/Memory/*.md 中重要內容批量匯入 DB memories（importance 4-5），匯入完成後將原始 .md 封存到 Knowledge/Archive/。屬於優化類。

### 📅 2026-04-10 [config] [待辦] 微調 Memory Writer importance 判斷規則
status=pending。微調 Memory Writer 的 importance 自動判斷規則，避免測試記憶被誤判成 importance=5。屬於優化類。

### 📅 2026-04-10 [feature] [待辦] MEMORY_PUBLIC.md 加入「今日 Daily Debrief 摘要」區塊
status=pending。MEMORY_PUBLIC.md Step 5 格式升級尚未完整，需加入「今日 Daily Debrief 摘要」區塊。屬於優化類。

### 📅 2026-04-10 [config] [待辦] 舊 Knowledge .md 文件降級整理與封存
status=pending。把舊的 Knowledge .md 文件降級整理，靜態保留 5-6 個核心文件，其餘封存至 Archive/。屬於下一個大功能。

### 📅 2026-04-10 [feature] [待辦] 評估 Jarvis Daily Debrief Telegram 通知格式優化
status=pending。評估是否需要加入 Jarvis Daily Debrief 的 Telegram 通知格式優化，提升可讀性與資訊密度。屬於下一個大功能評估。

### 📅 2026-04-10 [general] 哈哈，嚇到我們了 😅
哈哈，嚇到我們了 😅 今天修了不少坑——no-access 無限迴圈、記憶雜訊、比價通知沒觸發⋯⋯ 都是我們的問題，感謝主人有耐心撐過來 🙏 以後諸葛亮會主動盯盤，賈維斯也會記得你說的事。再見 是絕對不會說的！💪

### 📅 2026-04-10 [bug_fix] 修正 Mutual-Watchdog no-access 無限循環問題
修改檔案：Operations/Health/Mutual-Watchdog.py 問題：送出 /login 後沒有冷卻，每分鐘重複偵測 no-access 並重複通知諸葛亮。 修正內容： 1. 送出 /login 後，state 記錄 no_access_login_sent_ts，冷卻 10 分鐘內完全跳過 no-access 偵測 2. 新增 state 欄位 no_access_det...

### 📅 2026-04-10 [bug_fix] 修正記憶系統雜訊過濾與 DB 清理
修改檔案： 1. Operations/System/Jarvis_Memory_Writer.py — from_hook() 加入 _should_skip() 過濾邏輯 2. Operations/Sync/sync_claude_memory.sh — task_logs 查詢加 task_title NOT NULL + length >= 30 條件 過濾規則（Jarvis_Memo...

### 📅 2026-04-10 [infra] 清理完成
✅ 清理完成 刪除結果： - task_logs：刪 8 筆 → 剩 12 筆（全是真實任務） - memories：刪 12 筆 → 剩 17 筆（全是有意義記憶） 刪除的都是：Telegram bot reply、問候語、搜尋輸出等雜訊 sync 完成：6642 bytes（格式正確，每筆一個 ### 標題） 已 push GitHub + 部署 webhook。 確認格式： - 近兩...

### 📅 2026-04-10 [infra] sync 完成
✅ sync 完成 MEMORY_PUBLIC.md 7710 bytes（12:53 CST） 已 push GitHub + 部署 webhook 現在可以測試 fetch https://webhook.yison.io/memory.md 確認是否拿到最新版本（最後更新：2026-04-10 12:53 CST）

### 📅 2026-04-10 [infra] 兩個地方都修完了
✅ 兩個地方都修完了 【1】Telegram_Repair_Trigger.py — 兩處連結已更新： - 早安問候（發給賈維斯） - 打招呼偵測回應（諸葛亮回覆） → 連結改為：https://webhook.yison.io/memory.md → 諸葛亮已重啟 ✅ 【2】CLAUDE.md 打招呼規則 — 已更新（~/.claude/CLAUDE.md + yison_www/CLAUD...

### 📅 2026-04-10 [infra] 兩個小問題都修完了
✅ 兩個小問題都修完了 【1】雜訊記憶已刪：memories id=41「no-cache header 已套用」已從 DB 移除 【2】使用說明連結已更新： https://webhook.yison.io/memory.md sync 完成，已 push GitHub + 部署 webhook（13:00 CST） 明天早安測試期待！😊

### 📅 2026-04-10 [infra] 找到原因了
🔍 找到原因了 【根本問題】 漲跌 3% 通知是在 /api/watchlist/prices 被呼叫時才觸發， 但目前沒有任何排程主動去呼叫這個 API。 也就是說：只有當你打開 stock.yison.io/watchlist 頁面， 前端才會 fetch 一次，通知才有機會發出。 沒人看頁面 → 沒有觸發 → 沒有通知。 【現有排程】 crontab 有：stock_radar、new...

### 📅 2026-04-10 [bug_fix] 對不起，設計有問題！已修好。
對不起，設計有問題！已修好。 【問題】通知只在你開網站時才觸發，沒有主動輪詢機制。 【修正】已加入 crontab 排程： - 週一至週五 09:00–14:00，每 30 分鐘自動檢查一次 - 14:00 再多跑一次（收盤前） 從下個交易日起，只要自選股漲跌超過 3%，諸葛亮就會主動通知你，不需要開網站。 ⚠️ 目前自選股清單是空的（API 回傳 []），請確認 stock.yison....

### 📅 2026-04-10 [bug_fix] 找到真正原因了！API 設計有 bug——需要前端傳 symbols 參數才會查，不是自己從 DB 讀，所以 cron curl 拿到的永遠是空的 😅
找到真正原因了！API 設計有 bug——需要前端傳 symbols 參數才會查，不是自己從 DB 讀，所以 cron curl 拿到的永遠是空的 😅 已寫一支新腳本 watchlist_alert.py，直接連 DB 讀股票、抓價格、發通知。 剛才測試結果： 📈 華電網（6163.TWO）+3.36% → 通知已發 ✅ 📈 邁科（6831.TW）+3.11% → 通知已發 ✅ 台積電 +1....

### 📅 2026-04-10 [bug_fix] 自選股漲跌通知修正：新增 watchlist_alert.py 獨立腳本
問題：/api/watchlist/prices 需要前端傳 symbols 參數，cron curl 永遠拿到空陣列，通知從未觸發。 修正：新建 Web-Services/Stock-Dashboard/scripts/watchlist_alert.py - 直接連 stock_postgres DB 讀 watchlist 表 - 用 yfinance 抓即時價格 - 漲跌 >= 3% 或...


---

## 未完成事項

（目前無未完成事項）

---

## 重要長期記憶

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

### ⭐5 stock_dashboard資料庫配置
stock_dashboard 使用 PostgreSQL，port 5433，用途為儲存每週 AI 選股競技結果。週次 key 以本週一日期字串（YYYY-MM-DD）作為唯一識別，防止重複寫入。連線使用 psycopg2，採用 RealDictCursor。


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

### Docker 容器
- relaxed_carson: Up About an hour
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
