---
title: "Self Hosted Llm Macminim4"
date: 2025-01-25T22:46:43+09:00
draft: false
---

## Brief View

1. LM Studio
2. Open WebUI
3. CloudFlare tunnel

- > [!IMPORTANT]
>
> All setup based on Mac Mini M4

## LM Studio

- download from official website
- enable local server mode, default on `localhost:1234`

```bash
lms server start
```

## Open WebUI

- > [!NOTE]
>
> not support python3.13 at the time of writing

```bash
python3 -m venv ~/.venv/openwebui
source ~/.venv/openwebui/bin/activate
pip install open-webui
nohup open-webui sevrer > open-webui.log 2>&1 &
```

- Go to lcocalhost:8080, register admin account
- In `Settings` > `Admin settings` > `Connections` > `Manage OpenAI API Connections`,
set `URL` to `http://127.0.0.1:1234/v1` and `key` to `none`.
- Verify connections to see if LM Studio models are available.

## CloudFlare Tunnel

- prerequisites: cloudflare account, domain name

- I used namesilo before and switched to porkbun this time.

```bash

- If not installed, install by `brew install cloudflared`

```bash
cloudflared login
```

- Go to prompted URL to finish login.

```bash
cloudflared tunnel create <tunnel_name>
```

- It will generate a json file in `~/.cloudflared/` directory.

### Config yaml

- Put in `~/.cloudflared/config.yml`

```yaml
tunnel: <tunnel_name>
credentials-file: /Users/<user>/.cloudflared/<LONG-UUID>.json

ingress:
  - hostname: <hostname>
    service: http://localhost: <open_webui_port>
  # Default catch-all rule
  - service: http_status:404
```

- open_webui_port is default to 8080

- run in background

```bash
nohup cloudflared tunnel run <tunnel_name> > cloudflare_tunnel.log 2>&1 &
```
