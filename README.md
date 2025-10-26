# ☁️ Valheim Dedicated Server on Azure (Dockerized)

A hands-on DevOps and cloud deployment project: a self-hosted **Valheim** dedicated server deployed on a **Linux Azure VM**, containerized via **Docker Compose**. This project demonstrates real-world Azure administration, container orchestration, network security configuration, and troubleshooting within a persistent cloud environment.

---

## 💡 Why This Project
I built this project to merge my technical background in cloud operations with something interactive and practical. The goal was to create a persistent, multiplayer-ready game server using **Azure infrastructure** and **Docker** to focusing on automation, permissions management, networking, and long-term reliability. It became a fun sandbox for testing real DevOps workflows: provisioning, deploying, monitoring, and iterating quickly.

---

## 🧩 Stack Overview
- **Azure Virtual Machine (Ubuntu 24.04 LTS)**  
  - Provisioned via the Azure Portal with SSH key authentication.
  - Configured **Network Security Group (NSG)** rules for inbound **UDP 2456–2458** traffic.
  - Persistent data volumes mounted to ensure world continuity across container restarts.

- **Docker + Docker Compose**  
  - Image: [`lloesche/valheim-server`](https://hub.docker.com/r/lloesche/valheim-server)
  - Explicit `-savedir` argument to control world data storage.
  - Volume bindings for `/config` and `/steam`.
  - Environment variables for server name, password, and timezone.

- **Local Administration**
  - SSH access via PEM key for secure terminal management.
  - Container lifecycle managed with `docker-compose`.
  - Log monitoring and live updates through `docker logs -f valheim`.

---

## ⚙️ Deployment Steps

### 1. Launch Azure VM
```bash
# Create and SSH into your VM
ssh -i "valheimserver_key.pem" azureuser@<your-public-ip>
```

### 2. Install Docker and Docker Compose
```bash
sudo apt update
sudo apt install docker.io docker-compose -y
```

### 3. Deploy the Server
```bash
git clone https://github.com/lehender/Valheim-Server.git
cd Valheim-Server
docker-compose up -d
```

Monitor startup:
```bash
docker logs -f valheim | grep "Game server connected"
```

---

## 🗺️ Persistent World Storage
World data is stored persistently under `/config/worlds_local/`.

Upload or replace world saves:
```bash
scp -i "valheimserver_key.pem" alcoholwipes_server.db alcoholwipes_server.fwl \
    azureuser@<vm-ip>:/home/azureuser/valheim/config/worlds_local/
```
Then restart:
```bash
docker-compose down && docker-compose up -d
```

---

## 🧠 Troubleshooting Notes
- **Port Conflicts:** Fixed by adjusting NSG and local UFW rules to allow UDP 2456–2458.  
- **Permission Errors:** Resolved via `chown -R 1000:1000 /config` ensuring container access to mounted volumes.  
- **Wrong World Loads:** Fixed with explicit `-savedir /config/worlds_local` argument to prevent Valheim from generating duplicate worlds.  
- **Authentication Failures:** Managed with proper SSH key permissions (`icacls` on Windows / `chmod 400` on Linux).  
- **Container Restarts:** Enabled persistence with `restart: unless-stopped` directive in Compose.

These challenges provided hands-on experience diagnosing Linux permissions, Azure networking, and Docker volume persistence — a great practical exercise in end-to-end system ownership.

---

## 🌍 Connect in Valheim
```
IP: <azure-public-ip>:2456
Password: Hogna77
```

---

## 🧾 Key Learnings & Takeaways
- Applied Azure fundamentals: VM provisioning, inbound port rules, NSG configuration.
- Strengthened Docker proficiency: Compose orchestration, container networking, persistent volumes.
- Gained real troubleshooting experience: file permissions, live log inspection, and service restarts.
- Documented an entire deployment pipeline — from cloud infrastructure to application runtime.

---

**Author:** Levi Henderson  
**GitHub:** [@lehender](https://github.com/lehender)  
**Project:** [Valheim Server on Azure](https://github.com/lehender/Valheim-Server)
