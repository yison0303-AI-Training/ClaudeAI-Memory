# Yison-AI Server — 賈維斯動態記憶
> 由 sync_claude_memory.sh 自動產生，每日 03:05 更新
> 最後更新：2026-04-10 12:41 CST
> 任務總數（14天）：18 條｜未完成：0 條｜重要記憶：13 條
> ⚠️ 此檔案不含任何密碼、Token 或機敏 IP

---

## 今日摘要（Daily Debrief）

賈維斯今日完成MCP伺服器測試記憶及5個工具的建立與運作，並實作五模組記憶系統，涵蓋資料庫升級、記憶寫入器與查詢工具等核心功能。記錄中未提及修復項目與重要決策。


---

## 近兩週任務記錄

### 📅 2026-04-10 [bug_fix] 三項修正全部完成
✅ 三項修正全部完成 【修正一】Jarvis_Memory_Writer.py 加過濾： - 內容 < 30 字 → 跳過 - 問候語開頭（早安/午安/晚安/中午好/你好/hi/hello）→ 跳過 - 純表情符號或純網址 → 跳過 - title 無有效中英文字元 → 跳過 【修正二】DB 清理結果： - task_logs：清除 5 筆，剩 16 筆 - memories：清除 8 筆，剩...

### 📅 2026-04-10 [feature] MCP Server 測試記憶
jarvis-memory-mcp.py 建立完成，5個工具正常運作

### 📅 2026-04-10 [feature] search_memory「stock」查到 1 筆記錄：
✅ search_memory「stock」查到 1 筆記錄： 📌 [記憶 #8] ⭐ 重要度 5（永久保留） 類型：feature 標題：stock.yison.io 週選股雙AI競技架構 時間：2026-04-10 內容： stock.yison.io 週選股競技場採用雙AI架構： • Gemini（自由選）→ Claude（避免Gemini重複）→ Combined（黑馬，避免前10支）...

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

### 📅 2026-04-10 [feature] Rate Limit 自動偵測已完整實作！（2026-04-10 已加入）
✅ Rate Limit 自動偵測已完整實作！（2026-04-10 已加入） 確認 5 個功能全部到位： 1️⃣ 每 1 分鐘掃 tmux pane（crontab 輪詢），搜尋關鍵字：    • "You've hit your limit" / "hit your limit" / "rate-limit-options" / "resets" 2️⃣ 偵測到 → 自動送 3 + En...

### 📅 2026-04-10 [bug_fix] 修正 Mutual-Watchdog no-access 無限循環問題
修改檔案：Operations/Health/Mutual-Watchdog.py 問題：送出 /login 後沒有冷卻，每分鐘重複偵測 no-access 並重複通知諸葛亮。 修正內容： 1. 送出 /login 後，state 記錄 no_access_login_sent_ts，冷卻 10 分鐘內完全跳過 no-access 偵測 2. 新增 state 欄位 no_access_det...

### 📅 2026-04-10 [bug_fix] 已修正，mutual-watchdog 重啟完成
✅ 已修正，mutual-watchdog 重啟完成 修正摘要： 1. /login 後冷卻 10 分鐘，期間完全跳過 no-access 偵測 2. 狀態變化才通知：偵測到 no-access 通知一次、恢復後通知一次 3. /login 失敗（需瀏覽器授權）→ 通知「需要手動授權」並停止重試 完成通知已透過諸葛亮發送。

### 📅 2026-04-10 [bug_fix] 四步驟結果如下：
✅ 四步驟結果如下： 1. sync 腳本執行結果：    無錯誤，正常完成    - MEMORY_PUBLIC.md 寫入 14365 bytes    - TODO_CLAUDE.md 寫入    - Push 到 GitHub 成功 2. 有無錯誤：無，全程 ✅ 3. GitHub 最近 3 筆 commit：    247030c  auto: sync DB memory 202...

### 📅 2026-04-10 [infra] 已完成，memory.md 可直接 fetch
✅ 已完成，memory.md 可直接 fetch 修改內容： 1. Webhook src/index.js 加了 GET /memory.md 路由 2. sync_claude_memory.sh 最後加了 cp 步驟，自動部署到 Webhook/public/memory.md fetch 網址： https://webhook.yison.io/memory.md 確認： - ht...

### 📅 2026-04-10 [bug_fix] 修正記憶系統雜訊過濾與 DB 清理
修改檔案： 1. Operations/System/Jarvis_Memory_Writer.py — from_hook() 加入 _should_skip() 過濾邏輯 2. Operations/Sync/sync_claude_memory.sh — task_logs 查詢加 task_title NOT NULL + length >= 30 條件 過濾規則（Jarvis_Memo...

### 📅 2026-04-09 [feature] 賈維斯記憶系統實作
實作 5 模組記憶系統：DB schema 升級、Memory Writer、Claude Code Hook、Warmup 升級、諸葛亮記憶查詢工具


---

## 未完成事項

（目前無未完成事項）

---

## 重要長期記憶

### ⭐5 Mutual-Watchdog 自動修復機制
Mutual-Watchdog.py 自動修復機制：1. Rate limit：偵測→傳"3"→解析reset時間→寫state file→時間到自動重啟Jarvis。2. No-access（OAuth 8小時過期）：偵測→自動/login→選1→通知諸葛亮。3. 全域鎖避免重複執行。每分鐘由crontab觸發。

### ⭐5 賈維斯記憶系統五模組架構
賈維斯記憶系統五模組架構：1. DB schema（task_logs + memories 兩表）、2. Jarvis_Memory_Writer.py（同時寫兩表，hook觸發）、3. Claude Code Hook（PostToolUse on telegram reply）、4. Jarvis_Warmup.py（啟動時注入DB記憶到對話上下文）、5. search_jarvis_mem...

### ⭐5 4440.TWO 青雲電子 yfinance 衝突問題
4440.TWO 青雲電子在 yfinance 查詢時會與其他股票衝突，不要加入選股候選清單。已從 weekly_picks.py 的候選清單中排除。

### ⭐5 台股漲跌顏色規則（紅漲綠跌）
台股漲跌顏色規則與台灣習慣相反：漲用紅色（tw-positive）、跌用綠色（tw-negative）。前端 CSS class 須使用 tw-positive/tw-negative，不能用 gain/loss 或 green/red 直接命名。

### ⭐5 stock.yison.io 週選股雙AI競技架構
stock.yison.io 週選股競技場採用雙AI架構：Gemini（自由選）→ Claude（避免Gemini重複）→ Combined（黑馬，避免前10支）。三組各選5支，限制成交額150元以下台股，不得重複。每週五收盤後結算，比較漲跌幅準確率。

### ⭐4 Rate Limit 自動偵測已完整實作！（2026-04-10 已加入）
✅ Rate Limit 自動偵測已完整實作！（2026-04-10 已加入） 確認 5 個功能全部到位： 1️⃣ 每 1 分鐘掃 tmux pane（crontab 輪詢），搜尋關鍵字：    • "You've hit your limit" / "hit your limit" / "rate-limit-options" / "resets" 2️⃣ 偵測到 → 自動送 3 + En...

### ⭐4 中午好，主人！🌞
中午好，主人！🌞 📋 提醒：請將以下連結貼給 Claude AI，讓他同步最新記憶： https://raw.githubusercontent.com/yison0303-AI-Training/ClaudeAI-Memory/main/MEMORY_PUBLIC.md 今天是 2026-04-10，星期五！ 正在查 Mutual-Watchdog.py 是否已實作 rate limit ...

### ⭐4 兩處修改完成！
✅ 兩處修改完成！ 【1】諸葛亮問候語已更新 📄 Operations/Health/Telegram_Repair_Trigger.py (L940) 新訊息： 「早安，主人！☀️ 📋 提醒：請將以下連結貼給 Claude AI，讓他同步最新記憶： https://raw.githubusercontent.com/yison0303-AI-Training/ClaudeAI-Memory/...

### ⭐4 [待辦] 舊 Knowledge .md 文件降級整理與封存
status=pending。把舊的 Knowledge .md 文件降級整理，靜態保留 5-6 個核心文件，其餘封存至 Archive/。屬於下一個大功能。

### ⭐4 [待辦] MEMORY_PUBLIC.md 加入「今日 Daily Debrief 摘要」區塊
status=pending。MEMORY_PUBLIC.md Step 5 格式升級尚未完整，需加入「今日 Daily Debrief 摘要」區塊。屬於優化類。


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
- quirky_euler: Up 17 minutes
- stock_postgres: Up 47 hours (healthy)
- vaultwarden: Up 47 hours (healthy)
- yison-mcp-postgres: Up 47 hours
- yison-postgres-agent: Up 47 hours (healthy)
- yison-mcp-google-maps: Up 47 hours
- cloudflare_tunnel: Up 47 hours
- gallery_postgres: Up 47 hours (healthy)
- pgadmin: Up 47 hours

---

## 使用說明

將此連結的 raw 內容貼給 Claude AI 作為上下文記憶：
`https://raw.githubusercontent.com/yison0303-AI-Training/ClaudeAI-Memory/main/MEMORY_PUBLIC.md`
