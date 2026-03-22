# ARCHITECTURE.md — 系統架構總覽
# 最後更新：2026-03-22 09:05 CST（賈維斯 OAuth no-access 自動修復機制）

> 這份文件是 Claude Code 的核心參考，所有服務細節都在這裡。
> 修改服務前，必須先確認這裡的路徑和設定。

---

## 一、目錄結構（最終）

```
yison_www/
├── Web-Services/           ← 對外 HTTP 服務
│   ├── Gallery/            → gallery.yison.io  (Flask, port 5000)
│   ├── Webhook/            → webhook.yison.io  (Express, port 3020)
│   └── Salary-Dashboard/   → salary.yison.io   (Flask, port 5002)
│
├── Bots/                   ← AI 背景代理
│   ├── Assistant/          → Telegram AI 助手（原 tools/Assistant）
│   └── X-Daily-News/       → X 平台每日摘要（原 tools/X-Digest）
│
├── Utils/                  ← 命令列小工具
│   ├── Quick-Ask/
│   ├── Weather/
│   ├── World-Time/
│   ├── Text-To-Speech/
│   └── Google-Workspace-CLI/
│
├── Operations/             ← 維運腳本與排程（原 infra/automan）
│   ├── Cloudflare/         → Cloudflare Tunnel compose
│   ├── Health/             → 健康檢查、自動修復、Telegram Bot
│   ├── Sync/               → GitHub 同步、Env 同步
│   ├── System/             → 每日/每週維運腳本
│   ├── Network/            → 網路工具
│   ├── Log/                → 維運 log 集中目錄
│   └── Daily-Log/          → 每日維運日誌
│
├── Credentials/            ← 機敏資料集中管理（.env.master）
├── Knowledge/              ← 系統知識庫、文件、Claude 記憶
├── CLAUDE.md
└── README.md
```

---

## 二、網路架構（流量走向）

```
外部流量（瀏覽器、API 呼叫）
    │
    ▼
Cloudflare（DNS + CDN + DDoS 防護）
    │
    ▼
Cloudflare Tunnel（唯一對外入口，無需開放防火牆 port）
container: cloudflare_tunnel（Docker, host network）
    │
    ▼
nginx（127.0.0.1 反向代理，port 80）
    │
    ├── gallery.yison.io      → localhost:5000  (Gunicorn)
    ├── webhook.yison.io      → localhost:3020  (Express)
    ├── salary.yison.io       → localhost:5002  (Gunicorn)
    └── x-twitter.yison.io    → 靜態 HTML（Bots/X-Daily-News/output/）
```

**重要規則：**
- Gunicorn 只綁 `127.0.0.1`，絕不改成 `0.0.0.0`
- Cloudflare Tunnel 使用 `network_mode: host`，可直接存取主機所有 localhost port
- SSL/TLS 由 Cloudflare 處理，nginx 只需 listen 80

---

## 三、服務一覽

| 服務名稱 | 網域 | Port | 管理工具 | 語言/框架 |
|---------|------|------|---------|-----------|
| ai-gallery | gallery.yison.io | 5000 | systemd --user | Python / Flask + Gunicorn |
| ai-gallery-worker | — | — | systemd --user | Python |
| ai-gallery-ai-worker | — | — | systemd --user | Python |
| salary-dashboard | salary.yison.io | 5002 | systemd --user | Python / Flask + Gunicorn |
| webhook-service | webhook.yison.io | 3020 | PM2 (id=0) | Node.js / Express |
| telegram-bot（諸葛亮） | — | — | PM2 (id=1) | Python |
| claude-channel（賈維斯） | — | — | systemd --user | Claude Code |

---

## 四、各服務詳細說明

### 4-1. Gallery（gallery.yison.io）

**功能：** AI 智慧相冊，管理 Google Drive 照片，支援 AI 分析

**目錄：** `Web-Services/Gallery/`

| 項目 | 路徑 |
|------|------|
| Flask 主目錄 | `Web-Services/Gallery/backend/` |
| 入口點 | `Web-Services/Gallery/backend/wsgi.py` |
| AI 分析 worker | `Web-Services/Gallery/backend/worker.py` |
| 掃描補漏 worker | `Web-Services/Gallery/backend/ai_worker.py` |
| 環境變數 | `Web-Services/Gallery/backend/.env` → `Credentials/gallery.env.master` |
| Log 目錄 | `Web-Services/Gallery/gallery_log/` |
| nginx 源檔 | `Web-Services/Gallery/nginx/gallery.yison.io` |
| nginx 系統 | `/etc/nginx/sites-available/gallery.yison.io` → 源檔 symlink |
| systemd 主服務 | `~/.config/systemd/user/ai-gallery.service` |
| systemd worker | `~/.config/systemd/user/ai-gallery-worker.service` |
| systemd ai_worker | `~/.config/systemd/user/ai-gallery-ai-worker.service` |

**管理指令：**
```bash
systemctl --user {start|stop|restart|status} ai-gallery
systemctl --user {start|stop|restart|status} ai-gallery-worker
journalctl --user -u ai-gallery -f
```

---

### 4-2. Salary Dashboard（salary.yison.io）

**功能：** 薪資收入管理（華電聯網）：月總金額、季度彙總、收入明細

**目錄：** `Web-Services/Salary-Dashboard/`

| 項目 | 路徑 |
|------|------|
| Flask 主目錄 | `Web-Services/Salary-Dashboard/backend/` |
| 環境變數 | `Web-Services/Salary-Dashboard/backend/.env`（plain file，含 DB 設定） |
| Log 目錄 | `Web-Services/Salary-Dashboard/salary-dashboard_log/` |
| nginx 源檔 | `Web-Services/Salary-Dashboard/nginx/salary.yison.io` |
| systemd | `~/.config/systemd/user/salary-dashboard.service` |
| DB | `salary_dashboard`（在 gallery_postgres 容器，port 5432） |
| DB 帳號 | `salary_user` / 密碼見 Credentials |

**管理指令：**
```bash
systemctl --user {start|stop|restart|status} salary-dashboard
```

---

### 4-3. Webhook Service（webhook.yison.io）

**功能：** LINE Bot 後端（杯麵 Baymax），處理訊息、呼叫工具

**目錄：** `Web-Services/Webhook/`

| 項目 | 路徑 |
|------|------|
| 入口點 | `Web-Services/Webhook/src/index.js` |
| PM2 設定 | `Web-Services/Webhook/ecosystem.config.js` |
| 環境變數 | `Web-Services/Webhook/.env` → `Credentials/webhook.env.master` |
| Log 目錄 | `Web-Services/Webhook/webhook-service_log/` |

**Routes：**
- `POST /` (LINE webhook)
- `POST /api/ai-world-tts-summary`
- `POST /api/split-message`
- `POST /trigger/repair`
- `POST /trigger/update`
- `GET /health`

**引用的外部工具：**
- `Utils/Quick-Ask/ask.py`
- `Utils/World-Time/timezone_convert.py`
- `Utils/Text-To-Speech/Scripts/AI-World-Summary-TTS.py`
- `Bots/Assistant/skills/weather_report.py`

**管理指令：**
```bash
pm2 restart webhook-service
pm2 logs webhook-service
```

---

### 4-4. 諸葛亮 Bot（telegram-bot）

**PM2 name：** `telegram-bot`（id=1）
**腳本：** `Operations/Health/Telegram_Repair_Trigger.py`
**Bot Token：** `TELEGRAM_BOT_TOKEN`（Credentials/common.env.master）
**Chat ID：** `TELEGRAM_ZHUGE_CHAT_ID`

**職責（唯讀通知 + 輕量查詢）：**
- 接收所有自動通知（健康檢查、新聞、警告、開機報告）
- 天氣查詢、世界時間、待辦記事、Log 摘要
- 系統操作（修復/更新/重啟等）一律回覆「請透過賈維斯處理」

**架構：** Gemini 2.5 Flash + function calling（9 個工具）
```
使用者輸入 → 特殊指令攔截（重啟賈維斯直接執行）
  ↓
系統操作關鍵字攔截 → 重導向至賈維斯
  ↓
Gemini Round 1 → 執行工具 → Gemini Round 2
```

**9 個工具：**
| 工具 | 功能 |
|------|------|
| get_weather | 即時天氣（純文字，含國家/城市/氣溫/濕度/風速） |
| get_world_time | 世界時間查詢 / 時區換算 |
| get_system_health | 系統健康檢查報告 |
| get_log_summary | 每日 log 精華摘要 |
| get_health_trend | 7 天系統健康趨勢分析 |
| get_crontab_review | Crontab 排程審查建議 |
| save_note | 新增記事（Google Sheets） |
| complete_note | 標記記事完成 |
| list_notes | 列出未完成記事（附 ✅ 完成按鈕） |

**特殊指令：** 傳「重啟賈維斯」→ 直接執行 `systemctl --user restart claude-channel`

**重啟：** `pm2 restart telegram-bot`

---

### 4-5 補充. 賈維斯 Bot（Claude Code Channel）

**systemd：** `claude-code`（systemd --user，service 源頭：`Operations/Health/Claude-Code.Service`）
**重啟腳本：** `Operations/Health/Jarvis-Restart.sh`
**執行方式：** tmux session `claude`（`script -qfc` 模擬 TTY）
**Bot Token：** `TELEGRAM_JARVIS_BOT_TOKEN`（Credentials/common.env.master）
**Chat ID：** `TELEGRAM_JARVIS_CHAT_ID`
**Plugin：** `plugin:telegram@claude-plugins-official`

**職責（雙向操作）：**
- 系統修復、程式開發、服務管理、系統更新、複雜任務執行
- 接收使用者指令，Claude Code AI 執行後回報結果

**啟動流程：**
1. systemd 啟動 `Jarvis-Restart.sh`
2. 背景啟動 `Jarvis_Warmup.py`（delay 10 秒，發記憶注入 + 問候語）
3. 啟動報告由 `Jarvis_Restart_Report.py` 發送至諸葛亮（7 項檢查）

**OAuth Token 機制（重要）：**
- Claude Code 的 access token 每 **8 小時**過期
- 重啟後不會自動更新，需明確執行 `/login` 觸發 refresh
- `Jarvis-Restart.sh` **不含**自動 /login（已移除，避免時序競爭）
- 自動修復由 `Mutual-Watchdog.py` 統一負責（見下方）

**no-access 自動修復（Mutual-Watchdog.py）：**
- 每 5 分鐘偵測 tmux pane 輸出，搜尋 no-access 關鍵字
- 偵測到 → 立刻通知諸葛亮：「⚠️ 賈維斯偵測到 no-access，正在自動修復...」
- 自動送出 `/login` + `1`（Claude Pro）完成認證
- 修復成功 → 通知諸葛亮：「✅ 賈維斯已自動恢復正常」
- 修復失敗 → 通知諸葛亮並附手動指令：`tmux attach -t claude` → 輸入 `/login`

**特殊指令：** 傳「重啟諸葛亮」→ Claude Code 執行 `pm2 restart telegram-bot`

**管理：**
```bash
# 查看狀態
tmux has-session -t claude   # 0 = 存活

# 重啟
~/yison_www/Operations/Health/Jarvis-Restart.sh

# 手動 attach 排查
tmux attach -t claude
```

---

### 4-5. Assistant（Bots/Assistant）

**功能：** Telegram AI 助手，支援多種技能（天氣、系統管理、Google、記憶）

**目錄：** `Bots/Assistant/`（原 `tools/Assistant/`）

| 項目 | 路徑 |
|------|------|
| 主程式 | `Bots/Assistant/telegram_bot.py` |
| 技能目錄 | `Bots/Assistant/skills/` |
| 早安問候 | `Bots/Assistant/skills/morning_greeting.py` |
| 更新確認 | `Bots/Assistant/skills/update_check.py` |
| 環境變數 | `Bots/Assistant/.env` → `Credentials/assistant.env.master` |
| Log 目錄 | `Bots/Assistant/logs/` |
| 記憶目錄 | `Bots/Assistant/memory/` |

---

### 4-6. X-Daily-News（Bots/X-Daily-News）

**功能：** 自動抓取 X/Twitter 推文，整理每日摘要，推送通知

**目錄：** `Bots/X-Daily-News/`（原 `tools/X-Digest/`）

| 項目 | 路徑 |
|------|------|
| 主程式 | `Bots/X-Daily-News/run_digest.py` |
| 抓取腳本 | `Bots/X-Daily-News/scripts/pw_fetch_tweets.py` |
| 推送腳本 | `Bots/X-Daily-News/scripts/push_notify.py` |
| 環境變數 | `Bots/X-Daily-News/.env` → `Credentials/x-daily-news.env.master` |
| Log 目錄 | `Bots/X-Daily-News/x-digest_log/` |
| nginx 源檔 | `Bots/X-Daily-News/nginx/x-twitter.yison.io` |

---

### 4-7. Cloudflare Tunnel

**目錄：** `Operations/Cloudflare/`
**container：** `cloudflare_tunnel`（Docker, host network, restart: always）
**守護：** crontab 每 5 分鐘確認容器在跑，掛了自動重啟

---

## 五、Docker 容器一覽

| 容器名稱 | Image | Port | 用途 |
|---------|-------|------|------|
| cloudflare_tunnel | cloudflare/cloudflared | — | 對外唯一入口 |
| gallery_postgres | postgres:16-alpine | 5432 | ai_gallery + salary_dashboard DB |
| yison-postgres-agent | postgres:16-alpine | 5435 | claude_agent DB |
| pgadmin | dpage/pgadmin4 | 5050 | DB 管理介面（本機用） |
| yison-mcp-postgres | db-mcp-postgres | — | MCP PostgreSQL 工具 |
| yison-mcp-google-maps | db-mcp-google-maps | — | MCP Google Maps 工具 |
| competent_wiles | ghcr.io/github/github-mcp-server | 8082 | MCP GitHub 工具 |

---

## 六、資料庫

| DB 名稱 | 用途 | 帳號 | Port | 容器 |
|---------|------|------|------|------|
| ai_gallery | Gallery 照片管理 | ai_gallery_user | 5432 | gallery_postgres |
| salary_dashboard | 薪資收入記錄 | salary_user | 5432 | gallery_postgres |
| claude_agent | Assistant AI 記憶 | agent_admin | 5435 | yison-postgres-agent |

**超級使用者（gallery_postgres）：** `ai_gallery_user`（同時是 superuser）
**超級使用者密碼：** 見 `Credentials/gallery.env.master` 的 DB_PASSWORD

**連線：**
```bash
# gallery / salary DB（port 5432）
PGPASSWORD=[DB_CREDENTIALS] psql -h 127.0.0.1 -U ai_gallery_user -d ai_gallery
PGPASSWORD=[DB_CREDENTIALS] psql -h 127.0.0.1 -U salary_user -d salary_dashboard

# agent DB（port 5435）
psql -h 127.0.0.1 -p 5435 -U agent_admin -d claude_agent
```

### ai_gallery Schema
| Table | 用途 |
|-------|------|
| `albums` | 相簿列表 |
| `media_items` | 圖片/影片記錄 |
| `ai_analysis` | Gemini AI 分析結果 |
| `sync_runs` | Drive 同步執行記錄 |
| `sync_album_stats` | 每次同步的相簿統計 |

### salary_dashboard Schema
| Table | 用途 |
|-------|------|
| `income_entries` | 收入明細（月薪、津貼、獎金等） |
| `v_income_with_total` | View：含每月總計 |

---

## 七、機敏資料管理

```
Credentials/                          chmod 700
├── common.env.master                 chmod 600  ← 共用（Telegram/API Keys/Gmail）
├── gallery.env.master                chmod 600  ← Gallery DB + Gemini
├── webhook.env.master                chmod 600  ← LINE Bot Token
├── assistant.env.master              chmod 600  ← AI Assistant Bot
└── x-daily-news.env.master           chmod 600  ← X/Twitter 帳號

symlink 對應：
  Web-Services/Gallery/backend/.env   → Credentials/gallery.env.master
  Web-Services/Webhook/.env           → Credentials/webhook.env.master
  Bots/Assistant/.env                 → Credentials/assistant.env.master
  Bots/X-Daily-News/.env              → Credentials/x-daily-news.env.master
  Web-Services/Salary-Dashboard/backend/.env  ← plain file（獨立，含 DB 設定）
```

---

## 八、nginx 設定

| 服務 | 源檔（專案內） | 系統 sites-available | sites-enabled |
|------|-------------|---------------------|---------------|
| gallery.yison.io | `Web-Services/Gallery/nginx/gallery.yison.io` | symlink → 源檔 | symlink → sites-available |
| salary.yison.io | `Web-Services/Salary-Dashboard/nginx/salary.yison.io` | symlink → 源檔 | symlink → sites-available |
| x-twitter.yison.io | `Bots/X-Daily-News/nginx/x-twitter.yison.io` | symlink → 源檔 | symlink → sites-available |

```bash
sudo nginx -t && sudo nginx -s reload
```

---

## 九、排程總覽

| 時間 | 任務 | 腳本 |
|------|------|------|
| 每 5 分鐘 | Cloudflare Tunnel 守護 | crontab inline |
| 每 15 分鐘 | 系統監控 | `Operations/System/Monitor.sh` |
| 每 2 小時 | X 推文抓取 + 摘要 | `Bots/X-Daily-News/` |
| 02:00 | DB 備份 + 上傳 Drive | `Operations/System/Daily_DB_Backup.sh` |
| 03:00 | GitHub 同步 | `Operations/Sync/Github_Sync.sh` |
| 03:30 | Gallery 增量同步 | `Operations/System/Daily_Gallery_Sync.sh` |
| 06:00 | 早安問候 | `Bots/Assistant/skills/morning_greeting.py` |
| 07:50 | 每日記錄 | `Operations/System/Daily_Log.sh` |
| 08:00 | 健康檢查 | `Operations/Health/Run_Health_Check.sh` |
| 08:05 週一 | 健康趨勢分析 | `Operations/System/Weekly_Health_Trend_Analysis.py` |
| 09:00 週一 | Git 提交摘要 | `Operations/System/Weekly_Git_Summary.py` |
| 09:00 每月1日 | Crontab 審查 | `Operations/System/Monthly_Crontab_Review.py` |
| 12:00 | 套件更新確認 | `Bots/Assistant/skills/update_check.py` |
| 12:30 | X 日報推送 | `Bots/X-Daily-News/scripts/push_notify.py` |
| 22:00 | Log 精華摘要 | `Operations/System/Daily_Log_Summary.py` |
| 00:00 週日 | 全專案大檢查 | `Operations/System/Weekly_Codebase_Check.sh` |
| 01:00 週日 | Workspace 文件檢查 | `Operations/System/Weekly_Workspace_Doc_Check.py` |

---

## 十、技術版本

| 工具 | 版本 | 路徑 |
|------|------|------|
| Python 3（主要，linuxbrew） | 3.14.3 | `/home/linuxbrew/.linuxbrew/bin/python3` |
| Python 3.12（系統，worker 用） | 3.12.x | `/usr/bin/python3.12` |
| Node.js（linuxbrew） | 25.6.0 | `/home/linuxbrew/.linuxbrew/bin/node` |
| nginx | 1.24.0 | — |
| Docker | 29.2.1 | — |
| PM2 | 6.0.14 | `/home/linuxbrew/.linuxbrew/bin/pm2` |
| Gunicorn | 25.1.0 | — |

---

## 十一、服務重啟快速參考

```bash
# Gallery
systemctl --user restart ai-gallery ai-gallery-worker ai-gallery-ai-worker

# Salary Dashboard
systemctl --user restart salary-dashboard

# Webhook
pm2 restart webhook-service

# Telegram Bot
pm2 restart telegram-bot

# nginx
sudo nginx -t && sudo nginx -s reload

# Cloudflare Tunnel
cd /home/yison/yison_www/Operations/Cloudflare && docker compose restart
```

---

## 十二、服務重啟快速參考（含賈維斯/諸葛亮）

```bash
# 賈維斯（Claude Code Channel）
systemctl --user restart claude-channel

# 諸葛亮（Telegram Bot）
pm2 restart telegram-bot
```

---

## 十三、系統設定（2026-03-21）

### sudo 免密碼
允許免密碼執行（`/etc/sudoers.d/yison-nopasswd`）：
```
apt-get, reboot, systemctl, journalctl, service
```

### loginctl linger
已啟用（user systemd 服務開機自動啟動）：
```bash
loginctl enable-linger yison
```

### 開機自動啟動服務
| 服務 | 管理方式 | 備注 |
|------|---------|------|
| claude-channel | systemd --user | 賈維斯（Claude Code） |
| boot-notify | systemd --user | 開機通知，5 秒重試到網路就緒 |

---

## 十四、快速排障

```bash
# 確認所有服務狀態
systemctl --user status ai-gallery salary-dashboard claude-channel
pm2 list
docker ps

# 查 gallery log
tail -50 /home/yison/yison_www/Web-Services/Gallery/gallery_log/systemd.log

# 查 webhook log
pm2 logs webhook-service --lines 50

# 查 salary log
journalctl --user -u salary-dashboard -n 50

# 確認 Cloudflare Tunnel
docker ps | grep cloudflare_tunnel

# 確認網站可連
curl -s -o /dev/null -w "%{http_code}" https://gallery.yison.io
curl -s -o /dev/null -w "%{http_code}" https://salary.yison.io/api/entries
curl -s -o /dev/null -w "%{http_code}" https://webhook.yison.io/health
```
