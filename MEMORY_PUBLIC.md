# Yison-AI Server — 賈維斯動態記憶
> 由 sync_claude_memory.sh 自動產生，每日 03:05 更新
> 最後更新：2026-04-10 08:57 CST
> 任務總數（14天）：2 條｜未完成：0 條｜重要記憶：6 條
> ⚠️ 此檔案不含任何密碼、Token 或機敏 IP

---

## 今日摘要（Daily Debrief）

今日完成：MCP Server 測試記憶、賈維斯記憶系統實作


---

## 近兩週任務記錄

### 📅 2026-04-10 [feature] MCP Server 測試記憶
jarvis-memory-mcp.py 建立完成，5個工具正常運作

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
賈維斯記憶系統五模組架構：1. DB schema（task_logs + memories 兩表）、2. Jarvis_Memory_Writer.py（同時寫兩表，hook觸發）、3. Claude Code Hook（PostToolUse on telegram reply）、4. Jarvis_Warmup.py（啟動時注入DB記憶到對話上下文）、5. search_jarvis_memory 工具（Telegram Bot可查詢記憶）。

### ⭐5 4440.TWO 青雲電子 yfinance 衝突問題
4440.TWO 青雲電子在 yfinance 查詢時會與其他股票衝突，不要加入選股候選清單。已從 weekly_picks.py 的候選清單中排除。

### ⭐5 台股漲跌顏色規則（紅漲綠跌）
台股漲跌顏色規則與台灣習慣相反：漲用紅色（tw-positive）、跌用綠色（tw-negative）。前端 CSS class 須使用 tw-positive/tw-negative，不能用 gain/loss 或 green/red 直接命名。

### ⭐5 stock.yison.io 週選股雙AI競技架構
stock.yison.io 週選股競技場採用雙AI架構：Gemini（自由選）→ Claude（避免Gemini重複）→ Combined（黑馬，避免前10支）。三組各選5支，限制成交額150元以下台股，不得重複。每週五收盤後結算，比較漲跌幅準確率。

### ⭐4 MCP Server 測試記憶
jarvis-memory MCP Server 建立完成，5個工具正常


---

## 服務現況快照

### systemd 服務

- ai-gallery: active
- salary-dashboard: active
- stock-dashboard: active

### PM2 服務
- webhook-service: online
- telegram-bot: online
- mutual-watchdog: online

### Docker 容器
- dazzling_pare: Up 7 hours
- stock_postgres: Up 44 hours (healthy)
- vaultwarden: Up 44 hours (healthy)
- yison-mcp-postgres: Up 44 hours
- yison-postgres-agent: Up 44 hours (healthy)
- yison-mcp-google-maps: Up 44 hours
- cloudflare_tunnel: Up 44 hours
- gallery_postgres: Up 44 hours (healthy)
- pgadmin: Up 44 hours

---

## 使用說明

將此連結的 raw 內容貼給 Claude AI 作為上下文記憶：
`https://raw.githubusercontent.com/yison0303-AI-Training/ClaudeAI-Memory/main/MEMORY_PUBLIC.md`
