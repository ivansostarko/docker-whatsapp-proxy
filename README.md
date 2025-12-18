# WhatsApp Proxy (Docker Compose)

This repository runs a WhatsApp chat proxy using the official `facebook/whatsapp_proxy` image.

## What you need
1. A VPS with a PUBLIC IPv4 address.
2. (Recommended) DNS A-record pointing `proxy.example.com` -> your PUBLIC IP.
3. Inbound firewall rules open for TCP 80, 443, and/or 5222 (open at least 443 in most cases).
4. Docker + Docker Compose v2 installed.

## Files
- `docker-compose.yml` – service definition, ports, volumes
- `.env` – runtime configuration (you create this from `.env.example`)
- `config/` – local config assets (mounted read-only)

## Quick start
1) Create your `.env`:
```bash
cp .env.example .env
```

2) Edit `.env` and set **PUBLIC_IP**.

3) Start:
```bash
docker compose pull
docker compose up -d
```

4) Watch logs:
```bash
docker compose logs -f --tail=200
```

## Configure WhatsApp to use the proxy
On your phone:
- WhatsApp Settings -> Storage and Data -> Proxy
- Set Proxy host: your domain (recommended) or IP
- Port: typically 443 (if you exposed 443)

## Persistence (volumes)
This compose file creates:
- `wa_certs` mounted at `/etc/haproxy/certs`
- `wa_logs` mounted at `/var/log/haproxy`

List volumes:
```bash
docker volume ls | grep wa_
```

## Updating
If you leave `WHATSAPP_PROXY_IMAGE_TAG=latest`, updates can change behavior unexpectedly.
Prefer pinning a tag once you have a working deployment.

```bash
docker compose pull
docker compose up -d
docker compose logs -f --tail=200
```

## Optional: overriding HAProxy config (high risk)
Only do this after you have located and extracted the active config from the running container.

Locate likely paths:
```bash
docker exec -it whatsapp-proxy sh -lc 'ls -la /etc/haproxy /usr/local/etc/haproxy 2>/dev/null || true'
```

Copy out (example path only; may differ):
```bash
docker cp whatsapp-proxy:/usr/local/etc/haproxy/haproxy.cfg ./config/haproxy.cfg
```

Then uncomment the matching bind-mount line in `docker-compose.yml` and restart:
```bash
docker compose up -d
```

## Operational warnings
- Public proxies attract abuse/scanning. Monitor uptime and bandwidth.
- Do not expose any stats/metrics endpoints to the public internet without firewalling.
