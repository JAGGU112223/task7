# 🖥️ Task 7: Monitor System Resources Using Netdata

## 📌 Objective
Install **Netdata** and visualize system + application performance metrics in real-time using Docker.

---

## 🔗 Live Monitoring
You can monitor the system live here:  
**[Netdata Live Dashboard](http://13.203.204.86:19999/)**

*(Note: This is a live public link — anyone with this URL can view your server metrics.)*

---

## 🛠️ Tools Used
- **Netdata** (Free, open-source monitoring tool)
- **Docker**

---

## 🚀 Setup & Run

### 1️⃣ Install Docker
```bash
curl -fsSL https://get.docker.com | sh
sudo systemctl enable --now docker
docker --version
2️⃣ Open Port 19999
For UFW Firewall:
bash
Copy
Edit
sudo ufw allow 19999/tcp
For AWS EC2:
Go to Security Group of your instance

Add Inbound Rule:

Type: Custom TCP

Port: 19999

Source: 0.0.0.0/0 (or your IP for security)

3️⃣ Run Netdata via Docker
bash
Copy
Edit
docker run -d --name=netdata --restart unless-stopped \
  -p 19999:19999 \
  --cap-add SYS_PTRACE \
  --security-opt apparmor=unconfined \
  -v netdata-config:/etc/netdata \
  -v netdata-lib:/var/lib/netdata \
  -v netdata-cache:/var/cache/netdata \
  -v /etc/passwd:/host/etc/passwd:ro \
  -v /etc/group:/host/etc/group:ro \
  -v /proc:/host/proc:ro \
  -v /sys:/host/sys:ro \
  -v /etc/os-release:/host/etc/os-release:ro \
  -v /var/run/docker.sock:/var/run/docker.sock \
  netdata/netdata
4️⃣ Access the Dashboard
On same machine: http://localhost:19999

From another device: http://<your-public-ip>:19999

5️⃣ Explore Metrics
System Overview → CPU, Memory, Load, Swap

Disks → I/O, space usage

Network → Bandwidth usage

Containers → Per-container CPU & RAM (Docker charts)

Alerts → Go to Health → All Alerts for warnings/errors

6️⃣ View Logs
bash
Copy
Edit
docker logs netdata | tail -n 50
docker exec -it netdata bash
ls -lah /var/log/netdata
cat /var/log/netdata/error.log | tail -n 50
exit
7️⃣ Stop / Start / Remove
bash
Copy
Edit
docker stop netdata
docker start netdata
docker rm -f netdata
📷 Deliverables
Screenshot of Netdata dashboard showing:

CPU, Memory, Disk, Network, and Docker charts

(Optional) Screenshot of Health Alerts

🎯 Outcome
By completing this task, you will:

Understand lightweight, real-time server monitoring

Learn to view system and container metrics

Be able to detect performance issues quickly
