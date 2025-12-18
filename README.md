# WhatsApp Proxy (Docker Compose)

This deploys the official WhatsApp Chat Proxy container using Docker Compose.

## What this is (and what it is not)

- This is a **chat connectivity proxy** to help clients reach WhatsApp when direct access is blocked.
- Expect **public exposure**: if you share the proxy address, anyone you share it with can use it. If you publish it, strangers will use it. Plan accordingly (bandwidth, abuse, logs, legal risk).

Upstream reference implementation: https://github.com/WhatsApp/proxy

## Prerequisites

1) A Linux VPS/host with a **public IP**.
2) Docker + Docker Compose v2 (`docker compose version`).
3) Firewall/security group access to the required ports.

WhatsApp’s own guidance indicates your proxy host should have ports like **80, 443, or 5222** available and typically a domain/subdomain pointing at the server.  
(Practically: you can run IP-only, but domain DNS is strongly recommended operationally.)  
Sources: WhatsApp Help Center snippet and common deployment guidance. :contentReference[oaicite:1]{index=1}

## Quick start

From the folder containing `docker-compose.yml`:

```bash
docker compose pull
docker compose up -d
docker compose logs -f
