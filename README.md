# Terabox Downloader Bot

A Telegram Bot written in Python that downloads videos from Terabox and sends them directly to Telegram.

> **Architecture:** VPS bot → Replit Proxy API → Terabox CDN (bypasses datacenter IP blocks)

---

## Config Variables — `config.env`

| Variable | Description | Type |
|---|---|---|
| `BOT_TOKEN` | Telegram Bot Token from [@BotFather](https://t.me/BotFather) | `str` |
| `TELEGRAM_API` | API ID from <https://my.telegram.org> | `int` |
| `TELEGRAM_HASH` | API Hash from <https://my.telegram.org> | `str` |
| `FSUB_ID` | Force Subscribe Channel ID (starts with -100) | `int` |
| `DUMP_CHAT_ID` | Dump Channel ID — all videos forwarded here (starts with -100) | `int` |
| `USER_SESSION_STRING` | Pyrogram session string for 4GB uploads & faster speeds | `str` |
| `COOKIES` | Terabox cookie string (legacy, replaced by `NDUS_COOKIE`) | `str` |
| `NDUS_COOKIE` | Fresh Terabox `ndus` cookie value — **required for downloads** | `str` |
| `REPLIT_PROXY_URL` | URL of Replit proxy API endpoint | `str` |

---

## How It Works

```
User sends Terabox link
        ↓
VPS Bot calls Replit Proxy API
        ↓
Replit Proxy fetches dlink from dm.terabox.app (clean residential IP)
        ↓
VPS Bot downloads file via Replit streaming proxy
        ↓
File sent to Telegram
```

The Replit proxy is needed because Terabox blocks datacenter IPs (VPS). Replit acts as a middleman with a clean IP.

---

## VPS Deploy

### 1. Prerequisites

```bash
sudo apt install python3 python3-pip aria2
pip3 install -r requirements.txt
```

### 2. Clone & Configure

```bash
git clone https://github.com/MikoxYae/Terabox-Downloader-Bot/
cd Terabox-Downloader-Bot
cp config.env.example config.env
nano config.env
```

Fill in all required variables in `config.env`, including:

```env
NDUS_COOKIE=your_fresh_ndus_cookie_here
REPLIT_PROXY_URL=https://your-replit-domain.replit.dev/api/terabox
```

### 3. Run

```bash
python3 terabox.py
```

Or as background process:

```bash
nohup python3 terabox.py > bot.log 2>&1 &
```

---

## Getting Fresh Terabox Cookies (NDUS_COOKIE)

Terabox sessions expire periodically. When the bot stops downloading, refresh cookies:

1. Open **Kiwi Browser** on Android (supports DevTools)
2. Go to `dm.1024terabox.com` and **login**
3. Open a new tab → type `kiwi://inspect` → tap **inspect** on the Terabox tab
4. Go to **Application** → **Cookies** → `https://dm.1024terabox.com`
5. Find `ndus` cookie → copy its **Value**
6. Update `NDUS_COOKIE` in `config.env` on your VPS

---

## Updating the Bot

```bash
cd /root/Terabox-Downloader-Bot
git pull origin master
pkill -f terabox.py
sleep 2
python3 terabox.py &
```

---

## Support

Join support group: [**@JetMirror**](https://t.me/jetmirrorchatz)

