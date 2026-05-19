# 🚀 Home Assistant Server Monitor Dashboard

Simple and beautiful Home Assistant server monitoring dashboard.

This is PART 1 of the dashboard series.

Current version includes:

- 🌐 Internet Status
- 🔥 CPU Usage
- 🌡️ CPU Temperature
- 💾 Disk Usage
- 🧠 RAM Monitoring
- 📊 Network Traffic
- ⚡ Load Average
- ⏱️ Uptime
- 🖥️ Server Information

---

# 📦 STEP 1 — Install HACS Cards

Go to:

```text
HACS → Frontend
```

Install these cards:

```text
mini-graph-card
```

```text
mushroom cards
```

```text
card-mod
```

```text
button-card
```

Optional:

```text
apexcharts-card
```

---

# 🖥️ STEP 2 — Install System Monitor

Go to:

```text
Settings → Devices & Services → Add Integration
```

Search:

```text
SystemMonitor
```

Install integration.

---

# ✅ STEP 3 — Enable Required Entities

Go to:

```text
Settings → Devices & Services → Entities
```

Search:

```text
SystemMonitor
```

Enable these entities:

## CPU

✅ Processor temperature  
✅ Processor use

## Memory

✅ Memory free

(optional)

✅ Memory use

## Disk

✅ Disk use /  
✅ Disk use /config

## Load

✅ Load (1 min)  
✅ Load (5 min)  
✅ Load (15 min)

## Network

✅ Network throughput in eno1  
✅ Network throughput out eno1

## Server

✅ IPv4 address eno1  
✅ Last boot

---

# 🌐 STEP 4 — Add Internet Ping Sensor

Go to:

```text
Settings → Devices & Services → Add Integration
```

Search:

```text
Ping (ICMP)
```

Add:

```text
8.8.8.8
```

Name:

```text
Google Ping
```

Result entity:

```yaml
binary_sensor.8_8_8_8
```

---

# ⏱️ STEP 5 — Install Uptime Integration

Go to:

```text
Settings → Devices & Services → Add Integration
```

Search:

```text
Uptime
```

Install:

```text
Час безвідмовної роботи
```

Result entity:

```yaml
sensor.uptime
```

---

# 📊 Included Dashboard Features

✅ Internet Status  
✅ CPU Usage Graph  
✅ Network Traffic Graph  
✅ CPU Temperature  
✅ RAM Monitoring  
✅ Disk Usage  
✅ Load Average  
✅ Uptime  
✅ Server Info

---

# 📁 Recommended Repository Structure

```text
home-assistant-server-monitor/
│
├── dashboard/
├── images/
├── README.md
```

---

# 🔥 Stack Used

- Home Assistant
- HACS
- Mushroom Cards
- Mini Graph Card
- System Monitor
- Ping ICMP
- Uptime Integration

---

# 📺 Future Dashboard Parts

Planned next dashboards:

- 🐳 Docker Monitoring
- 📹 Frigate AI Dashboard
- 🤖 Telegram Alerts
- 🚗 Tesla Dashboard
- ☀️ Solar / Deye Dashboard
- ⚡ Intel GPU Monitoring

---

# 📥 Import Dashboard

Open Home Assistant dashboard.

Add card manually.

Paste contents from:

```text
server_monitor_1.yaml
```

Save dashboard.

Done ✅
