# Claude 全域設定 — Yison-AI Server
# 最後更新：2026-03-22 09:31 CST（精簡版）

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
| sudo password | [REDACTED] |
| Python3 | `/home/linuxbrew/.linuxbrew/bin/python3` |
| Node.js / PM2 | `/home/linuxbrew/.linuxbrew/bin/` |

詳細工具路徑 → `Knowledge/PATH_REFERENCE.md`

---

## Telegram Bot
| Bot | 重啟指令 | Token env | Chat ID env |
|-----|---------|-----------|-------------|
| 諸葛亮（通知） | `pm2 restart telegram-bot` | `TELEGRAM_BOT_TOKEN` | `TELEGRAM_ZHUGE_CHAT_ID` |
| 賈維斯（Claude） | `~/yison_www/Operations/Health/Jarvis-Restart.sh` | `TELEGRAM_JARVIS_BOT_TOKEN` | `TELEGRAM_JARVIS_CHAT_ID` |

- **所有修復/更新完成報告一律由諸葛亮發送**
- 賈維斯：tmux session `claude`，狀態 `tmux has-session -t claude`
- **no-access 自動修復**：Mutual-Watchdog.py 負責（OAuth 8 小時過期，自動 /login + 選 1）
- Jarvis-Restart.sh **不含**自動 /login（已移除，由 Watchdog 統一處理）

詳細架構 → `Knowledge/Memory/telegram_bots.md`

---

## 常用指令
```bash
systemctl --user {start|stop|restart|status} ai-gallery
systemctl --user {start|stop|restart|status} salary-dashboard
pm2 restart webhook-service
pm2 restart telegram-bot
sudo nginx -t && sudo nginx -s reload
docker ps
bash /home/yison/yison_www/Operations/Health/Run_Health_Check.sh
```

---

## GitHub
SSH alias：`training-github.github.com`（`~/.ssh/id_ed25519_training`）
4 個 Repo：Web-Services / Bots / Utils / Operations → `yison0303-AI-Training/`
**絕對不上傳：** `Credentials/`、`**/.env*`、`health_check.conf`、`repair_trigger_state`

詳細 → `Knowledge/GITHUB.md`

---

## Gemini AI 設計原則
查詢類→Search grounding / 分析類→文字生成 / 對話類→function calling / 生成類→Imagen

---

## 記憶索引（去哪找什麼）
| 找什麼 | 去哪裡找 |
|--------|---------|
| 服務架構、目錄、DB、nginx | `Knowledge/ARCHITECTURE.md` |
| 所有工具完整路徑 | `Knowledge/PATH_REFERENCE.md` |
| crontab 排程完整說明 | `Knowledge/CRONTAB_REFERENCE.md` |
| GitHub repo 對應、sync 說明 | `Knowledge/GITHUB.md` |
| API Key / Token 實際值 | `Credentials/common.env.master`（不上 GitHub） |
| DB 密碼 / 連線資訊 | `Credentials/gallery.env.master` + `Knowledge/Memory/databases.md` |
| Bot Token / Chat ID 環境變數 | `Credentials/common.env.master` → 上方表格欄位名稱 |
| Cloudflare Tunnel / nginx / SSL | `Knowledge/Memory/infrastructure.md` |
| 健康檢查、自動修復、互保 | `Knowledge/Memory/automation.md` |
| 排障記錄 | `Knowledge/Memory/troubleshooting.md` |
| Claude Code 最佳實踐 | `Knowledge/Memory/claude_code_tips.md` |
| Google OAuth / Drive / Gmail | `Knowledge/Memory/google_workspace.md` |
| 兩個 Bot 詳細職責 | `Knowledge/Memory/telegram_bots.md` |

---

## 行為準則
- 修改系統設定前先確認，不自動執行破壞性操作
- 有疑問先問，不猜測
- 完成任務後主動更新對應的記憶主題檔
- commit 前先掃描機敏資料，確認 .gitignore 規則
- 新功能優先考慮加入 Gemini AI
- 修復/更新完成報告一律透過諸葛亮 bot 發送
- 需要確認的計畫必須透過賈維斯 Telegram channel 發送
