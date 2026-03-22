# Yison-AI Server — 快速索引
> 詳細內容在各主題檔，Grep 可快速搜尋
> 最後更新：2026-03-22 09:05 CST（Claude Code Channel 更新：tmux + Watchdog 自動修復 login）

## 專案一覽
| 專案 | 網域 | Port | 管理 |
|------|------|------|------|
| ai-gallery | gallery.yison.io | 5000 (Gunicorn) | systemd: ai-gallery / ai-gallery-worker / ai-gallery-ai-worker |
| salary-dashboard | salary.yison.io | 5002 (Gunicorn) | systemd: salary-dashboard |
| webhook-service | webhook.yison.io | 3020 (Express) | PM2 id=3: webhook-service |
| telegram-bot | — | — | PM2 id=4: Telegram_Repair_Trigger.py --daemon |
| X-Daily-News | x-twitter.yison.io | — | crontab（每 2 小時） |
| Assistant | — | — | crontab（早安/更新確認） |

## 根目錄結構（最終）
```
yison_www/
├── Web-Services/   → Gallery / Webhook / Salary-Dashboard
├── Bots/           → Assistant / X-Daily-News
├── Utils/          → Quick-Ask / Weather / World-Time / TTS / GWS-CLI
├── Operations/     → Health / System / Sync / Cloudflare / Network / Log
├── Credentials/    → 所有 .env.master（不上 GitHub）
├── Knowledge/      → 系統文件、Claude 記憶
├── CLAUDE.md
└── README.md
```

## 最常用指令
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
```

## 關鍵路徑
- 根目錄：`/home/yison/yison_www/`
- Log：`Operations/Log/`
- nginx 源檔：`{service}/nginx/{domain}`
- Cloudflare Tunnel：`Operations/Cloudflare/docker-compose.yml`
- 健康檢查：`Operations/Health/Run_Health_Check.sh`
- 健康檢查設定：`Operations/Health/health_check.conf`（不上 GitHub）
- Telegram Bot：`Operations/Health/Telegram_Repair_Trigger.py`

## 系統資訊
- sudo password: [PASSWORD]
- Python3: `/home/linuxbrew/.linuxbrew/bin/python3`
- Node.js: `/home/linuxbrew/.linuxbrew/bin/node`
- PostgreSQL superuser: `ai_gallery_user`（port 5432，gallery_postgres 容器）
- DB 密碼：見 `Credentials/gallery.env.master` → DB_PASSWORD
- LINE Bot: 杯麵 Baymax，User ID: [LINE_USER_ID]

## 資料庫
| DB | 帳號 | Port | 容器 |
|----|------|------|------|
| ai_gallery | ai_gallery_user | 5432 | gallery_postgres |
| salary_dashboard | salary_user | 5432 | gallery_postgres |
| claude_agent | agent_admin | 5435 | yison-postgres-agent |

## Telegram Bot（2026-03-20 升級）
- 架構：Gemini 2.5 Flash + function calling（9 個工具）
- Safety gate：修復/更新 直接執行，其他自然語言走 AI
- 工具：get_weather / get_world_time / get_system_health / get_log_summary / get_health_trend / get_crontab_review / save_note / complete_note / list_notes
- list_notes 附帶 ✅ 完成按鈕（inline keyboard），callback 由 `_handle_callback()` 處理
- 重啟：`pm2 restart telegram-bot`
- **諸葛亮 token**：[BOT_TOKEN]（自動通知、系統報告）
- **賈維斯 token**：[BOT_TOKEN]（問候語、Claude Code 相關通知），存於 `Credentials/common.env.master` → `TELEGRAM_JARVIS_BOT_TOKEN`

## Claude Code Channel（2026-03-22 更新）
- 用途：透過 Telegram 遠端操控 Claude Code
- Plugin：`telegram@claude-plugins-official`
- 執行方式：**tmux**，session 名稱：`claude`
- 重啟：`~/yison_www/Operations/Health/Jarvis-Restart.sh`
- 查看狀態：`tmux has-session -t claude`（回傳 0 = 存活）
- 開機自動啟動：`claude-code.service`（systemd user service）
- **OAuth token 機制**：access token 每 8 小時過期，重啟後不自動更新
- **no-access 自動修復**：`Mutual-Watchdog.py` 每 5 分鐘偵測，發現 no-access → 自動送 `/login` + 選 `1`，並通知諸葛亮始末
- `Jarvis-Restart.sh` **不含**自動 /login（已移除，由 Watchdog 統一處理）

## GitHub（2026-03-20 重建）
| 本機目錄 | Repo | Remote |
|---------|------|--------|
| Web-Services/ | yison0303-AI-Training/Web-Services | training |
| Bots/ | yison0303-AI-Training/Bots | training |
| Utils/ | yison0303-AI-Training/Utils | training |
| Operations/ | yison0303-AI-Training/Operations | training |
- SSH alias: `training-github.github.com`（`~/.ssh/id_ed25519_training`）
- 絕對不上傳：`Credentials/`、`**/.env*`、`health_check.conf`、`repair_trigger_state`、`**/Daily-Archive/`

## Gemini AI 設計原則
新工具 / 新功能 盡量加 Gemini AI：查詢類用 Search grounding、分析類用文字生成、對話類用 function calling、生成類用 Imagen

## 主題檔索引（詳細內容）
| 檔案 | 內容 |
|------|------|
| `infrastructure.md` | Cloudflare Tunnel 路由、nginx config、log 路徑、SSL |
| `databases.md` | PostgreSQL、Docker DB、連線資訊 |
| `automation.md` | 健康檢查、crontab 完整說明、Telegram Bot 架構 |
| `troubleshooting.md` | 過去排查過的問題與解法 |
| `claude_code_tips.md` | Claude Code 最佳實踐、MCP 選擇、Hooks、Token 管理 |
| `google_workspace.md` | gws OAuth 憑證位置、Drive/Gmail 授權狀態與使用方式 |
| `feedback_database_user_rule.md` | 每個新服務需建立專屬 DB 管理帳號，不共用 |
| `feedback_root_directory_rule.md` | yison_www 根目錄只放 CLAUDE.md 和 README.md |
| `feedback_knowledge_update_rule.md` | 每次改完程式碼，主動提醒用戶確認是否更新 Knowledge |
| `telegram_bots.md` | 兩個 Bot 職責：諸葛亮收自動通知，賈維斯與 Claude Code 對話 |
| `feedback_report_via_zhuge.md` | 修復/更新完成的報告一律由諸葛亮發送，不在賈維斯 channel 直接報告 |
