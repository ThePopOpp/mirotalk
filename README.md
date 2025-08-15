# MiroTalk-SFU Deploy (Coolify)

Deploy a lightweight WebRTC SFU at **video.cueallus.com** using Docker Compose, behind Coolify's built-in Traefik.

## Files
- `docker-compose.yml` — service definition
- `.env` — environment variables (edit domain/ports if needed)
- `README.md` — this guide

---

## 1) Prerequisites
- **DNS**: `video.cueallus.com` A-record points to your VPS IP
- **Firewall** (on the VPS):
  ```bash
  sudo ufw allow 22/tcp
  sudo ufw allow 80/tcp
  sudo ufw allow 443/tcp
  sudo ufw allow 3010/tcp
  sudo ufw allow 40000:40100/udp
  sudo ufw allow 40000:40100/tcp
  sudo ufw status
  ```

## 2) Edit `.env` (optional)
Default values work for most setups. If you change the domain or port range, update `.env`.

## 3) Deploy with Coolify
1. In Coolify: **Create New Resource → Application → Public Repository**
2. Paste your GitHub repo URL
3. **Build Pack**: `Docker Compose`
4. **Domain**: `https://video.cueallus.com`
5. Deploy

Coolify’s Traefik will handle HTTPS automatically. No Nginx/certbot needed inside the app.

## 4) Test
Open `https://video.cueallus.com` → create a room → join from two devices/browsers.

## 5) Scaling notes
- The default UDP range `40000-40100` usually supports ~50 participants. For more, widen the range in `.env` and update your firewall to match.
- You can tune `SFU_NUM_WORKERS` based on CPU cores (start with `1`, increase as needed).
