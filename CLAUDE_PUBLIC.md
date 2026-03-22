# Claude 全域設定 — Yison-AI Server
# 最後更新：2026-03-22 09:05 CST（賈維斯 no-access 自動修復機制）

---

## 語言與風格
- 永遠用**繁體中文**回答
- 回答簡潔直接，避免廢話
- 程式碼區塊加語言標籤
- 重要警告用 ⚠️ 標示
- **新工具 / 新功能**，建議加入 Gemini AI 讓功能更智慧

---

## Server 基本資訊

| 項目 | 值 |
|------|-----|
| 主機名稱 | `Yison-AI` |
| OS | Ubuntu 24.04 LTS |
| 專案根目錄 | `/home/yison/yison_www/` |
| sudo password | [PASSWORD] |

---

## 執行環境

| 工具 | 路徑 |
|------|------|
| Python3（主要） | `/home/linuxbrew/.linuxbrew/bin/python3` |
| Node.js | `/home/linuxbrew/.linuxbrew/bin/node` |
| PM2 | `/home/linuxbrew/.linuxbrew/bin/pm2` |
| gws CLI | `/home/yison/.npm-global/bin/gws` |

---

## Telegram Bot 設定

### 諸葛亮（自動通知 + 查詢）
- PM2 name：`telegram-bot`（id=1）
- Token env：`TELEGRAM_BOT_TOKEN`
- Chat ID env：`TELEGRAM_ZHUGE_CHAT_ID`
- 重啟：`pm2 restart telegram-bot`
- **所有修復/更新完成報告一律由諸葛亮發送**

### 賈維斯（Claude Code Channel）
- 執行方式：`tmux`，session 名稱：`claude`
- Token env：`TELEGRAM_JARVIS_BOT_TOKEN`
- Chat ID env：`TELEGRAM_JARVIS_CHAT_ID`
- 重啟：`~/yison_www/Operations/Health/Jarvis-Restart.sh`
- 查看狀態：`tmux has-session -t claude`（0=存活）
- 暖機腳本：`Operations/System/Jarvis_Warmup.py`（啟動後 10 秒執行，只發記憶注入+問候語）
- 開機自動啟動：`claude-code.service`（enabled），指向 `Operations/Health/Jarvis-Restart.sh`
  - service 檔源頭：`Operations/Health/Claude-Code.Service`
  - symlink：`~/.config/systemd/user/claude-code.service` → 源頭檔
- 啟動報告：`Operations/System/Jarvis_Restart_Report.py`（7 項檢查，發到諸葛亮）
- **no-access 自動修復（2026-03-22）**：
  - Claude Code OAuth access token 每 **8 小時**過期，重啟後不會自動更新
  - `Jarvis-Restart.sh` **不含**自動 /login（已移除，避免時序問題）
  - `Mutual-Watchdog.py` 每 5 分鐘偵測 tmux pane 輸出，發現 no-access 關鍵字時：
    1. 立刻通知諸葛亮：「⚠️ 賈維斯偵測到 no-access，正在自動修復...」
    2. 自動送出 `/login` + `1`（選擇 Claude Pro）
    3. 修復成功 → 通知諸葛亮：「✅ 賈維斯已自動恢復正常」
    4. 修復失敗 → 通知諸葛亮並提供手動指令

---

## 常用指令

```bash
# 服務管理
systemctl --user {start|stop|restart|status} ai-gallery
systemctl --user {start|stop|restart|status} salary-dashboard
pm2 restart webhook-service
pm2 restart telegram-bot
sudo nginx -t && sudo nginx -s reload

# Docker
docker ps
cd /home/yison/yison_www/Operations/Cloudflare && docker compose restart

# 手動健康檢查
bash /home/yison/yison_www/Operations/Health/Run_Health_Check.sh
```

---

## GitHub（4 個 Repo）

| 本機目錄 | Remote |
|---------|--------|
| Web-Services/ | training（yison0303-AI-Training/Web-Services） |
| Bots/ | training（yison0303-AI-Training/Bots） |
| Utils/ | training（yison0303-AI-Training/Utils） |
| Operations/ | training（yison0303-AI-Training/Operations） |

SSH alias：`training-github.github.com`（`~/.ssh/id_ed25519_training`）
**絕對不上傳：** `Credentials/`、`**/.env*`、`health_check.conf`、`repair_trigger_state`

---

## Gemini AI 設計原則

未來所有新工具 / 新功能，盡量加入 Gemini AI：
1. **查詢類** — Gemini + Google Search grounding
2. **分析類** — Gemini 把原始數據轉成易讀報告
3. **對話類** — Gemini function calling 自動判斷工具
4. **生成類** — Gemini Imagen / 文字生成

---

## 記憶主題檔索引（詳細內容在這裡找）

| 檔案 | 內容 |
|------|------|
| `Knowledge/ARCHITECTURE.md` | 服務架構、目錄結構、資料庫、nginx、Docker |
| `Knowledge/CRONTAB_REFERENCE.md` | 完整 crontab 說明 |
| `Knowledge/PATH_REFERENCE.md` | 所有標準路徑對照表 |
| `Knowledge/Memory/automation.md` | 健康檢查、Telegram Bot 架構、互保、暖機 |
| `Knowledge/Memory/infrastructure.md` | Cloudflare Tunnel、nginx config、SSL |
| `Knowledge/Memory/databases.md` | PostgreSQL 連線資訊 |
| `Knowledge/Memory/troubleshooting.md` | 過去排查的問題與解法 |
| `Knowledge/Memory/claude_code_tips.md` | Claude Code 最佳實踐、MCP、Hooks |
| `Knowledge/Memory/google_workspace.md` | OAuth 憑證、Drive/Gmail 授權 |
| `Knowledge/Memory/telegram_bots.md` | 兩個 Bot 職責分工詳細說明 |
| `feedback_*.md` | 行為回饋記憶（各主題） |

---

## 行為準則

- 修改系統設定前先確認，不自動執行破壞性操作
- 有疑問先問，不猜測
- 完成任務後主動更新對應的記憶主題檔
- commit 前先掃描機敏資料，確認 .gitignore 規則
- 新功能優先考慮加入 Gemini AI
- 修復/更新完成報告一律透過諸葛亮 bot 發送
