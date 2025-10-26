# Valheim Server (Docker + Azure)

A simple, containerized Valheim dedicated server running on an Azure Linux VM.  
Powered by Docker Compose using the official [lloesche/valheim-server](https://hub.docker.com/r/lloesche/valheim-server) image.

---

## 🧩 Stack
- Ubuntu 24.04 (Azure VM)
- Docker + Docker Compose
- lloesche/valheim-server image

---

## ⚙️ Usage
```bash
# Start server
docker-compose up -d

# Stop server
docker-compose down

# Check logs
docker logs -f valheim
🗺️ World Files
Place your world saves under:

arduino
Copy code
valheim/config/worlds_local/
Then restart the container:

bash
Copy code
docker-compose down && docker-compose up -d
🧠 Notes
Default ports: 2456–2458/udp

Password: Hogna77

Edit docker-compose.yml to update world name, password, or mods.

Levi Henderson • GitHub @lehender
```
