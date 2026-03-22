# Yison-AI Server — 快速索引
> 詳細內容在各主題檔，Grep 可快速搜尋
> 最後更新：2026-03-22 09:31 CST（精簡版）

## 專案一覽
| 專案 | 網域 | Port | 管理 |
|------|------|------|------|
| ai-gallery | gallery.yison.io | 5000 (Gunicorn) | systemd: ai-gallery / ai-gallery-worker / ai-gallery-ai-worker |
| salary-dashboard | salary.yison.io | 5002 (Gunicorn) | systemd: salary-dashboard |
| webhook-service | webhook.yison.io | 3020 (Express) | PM2 id=3: webhook-service |
| telegram-bot | — | — | PM2 id=4: Telegram_Repair_Trigger.py --daemon |
| X-Daily-News | x-twitter.yison.io | — | crontab（每 2 小時） |
| Assistant | — | — | crontab（早安/更新確認） |

## 最常用指令
```bash
systemctl --user {start|stop|restart|status} ai-gallery
systemctl --user {start|stop|restart|status} salary-dashboard
pm2 restart webhook-service
pm2 restart telegram-bot
sudo nginx -t && sudo nginx -s reload
docker ps
cd /home/yison/yison_www/Operations/Cloudflare && docker compose restart
```

## 關鍵路徑
- 根目錄：`/home/yison/yison_www/`
- Log：`Operations/Log/`
- nginx 源檔：`{service}/nginx/{domain}`
- Cloudflare Tunnel：`Operations/Cloudflare/docker-compose.yml`
- 健康檢查：`Operations/Health/Run_Health_Check.sh`（設定：`health_check.conf`，不上 GitHub）

## 系統資訊
- sudo password: [REDACTED]
- Python3: `/home/linuxbrew/.linuxbrew/bin/python3`（詳細路徑 → `Knowledge/PATH_REFERENCE.md`）
- DB 密碼：見 `Credentials/gallery.env.master` → DB_PASSWORD
- LINE Bot User ID: [REDACTED]

## 資料庫
| DB | 帳號 | Port | 容器 |
|----|------|------|------|
| ai_gallery | ai_gallery_user | 5432 | gallery_postgres |
| salary_dashboard | salary_user | 5432 | gallery_postgres |
| claude_agent | agent_admin | 5435 | yison-postgres-agent |

詳細連線資訊 → `databases.md`

## Telegram Bot
- 諸葛亮：`pm2 restart telegram-bot`，Token env: `TELEGRAM_BOT_TOKEN`，Chat env: `TELEGRAM_ZHUGE_CHAT_ID`
- 賈維斯：`~/yison_www/Operations/Health/Jarvis-Restart.sh`，Token env: `TELEGRAM_JARVIS_BOT_TOKEN`，Chat env: `TELEGRAM_JARVIS_CHAT_ID`
- Gemini 2.5 Flash + function calling（9 個工具）
- no-access 自動修復：Mutual-Watchdog.py（OAuth 8 小時過期，自動 /login + 選 1，通知諸葛亮）

詳細架構 → `telegram_bots.md`

## Claude Code Channel（賈維斯）
- tmux session `claude`，狀態：`tmux has-session -t claude`（0=存活）
- 開機自動啟動：`claude-code.service`（systemd user service）
- no-access 修復：Watchdog 偵測 → 自動 /login → 通知諸葛亮

詳細 → `telegram_bots.md` / `automation.md`

## GitHub
- SSH alias: `training-github.github.com`（`~/.ssh/id_ed25519_training`）
- 4 Repos: Web-Services / Bots / Utils / Operations → `yison0303-AI-Training/`
- 絕對不上傳：`Credentials/`、`**/.env*`、`health_check.conf`、`repair_trigger_state`、`**/Daily-Archive/`

詳細 → `Knowledge/GITHUB.md`

## Gemini AI 設計原則
新工具 / 新功能 盡量加 Gemini AI：查詢類用 Search grounding、分析類用文字生成、對話類用 function calling、生成類用 Imagen

## 主題檔索引（找什麼 → 去哪裡）
| 找什麼 | 主題檔 |
|--------|-------|
| Cloudflare Tunnel / nginx / SSL | `infrastructure.md` |
| DB 密碼、Docker DB、連線字串 | `databases.md` |
| 健康檢查、crontab、Bot 架構 | `automation.md` |
| 排障記錄（過去問題與解法） | `troubleshooting.md` |
| Claude Code 最佳實踐、MCP、Hooks | `claude_code_tips.md` |
| Google OAuth / Drive / Gmail 授權 | `google_workspace.md` |
| 兩個 Bot 詳細職責分工 | `telegram_bots.md` |
| API Key / Token 實際值 | `Credentials/common.env.master`（不上 GitHub） |
