# 🚀 Splunk SSH Brute-Force Detection Setup

## 📌 Overview
This repository documents the setup of **Splunk Forwarder** on **Kali Linux VM (192.168.1.54)** to monitor **SSH login attempts** and forward logs to **Splunk Server**. Additionally, it includes a **dashboard** and an **alert system** to detect multiple failed login attempts.

---
## 🛠️ Setup & Configuration

### **1️⃣ Install and Configure Splunk Forwarder on Kali VM (192.168.1.54)**
```bash
# Download and Install Splunk Forwarder (if not installed)
sudo dpkg -i splunkforwarder-<version>.deb
```

#### **Check if Splunk Forwarder is Running**
```bash
sudo /opt/splunkforwarder/bin/splunk status
```

#### **List Monitored Files**
```bash
sudo /opt/splunkforwarder/bin/splunk list monitor
```

#### **Add SSH Log Monitoring**
```bash
sudo /opt/splunkforwarder/bin/splunk add monitor /var/log/auth.log
```

#### **Forward Logs to Splunk Server**
```bash
sudo /opt/splunkforwarder/bin/splunk add forward-server 127.0.0.1:9997
```

#### **Restart Splunk Forwarder**
```bash
sudo /opt/splunkforwarder/bin/splunk restart
```

---
## **2️⃣ Setup SSH Brute Force Attempts on Windows VM (192.168.1.53)**

### **Install PuTTY for SSH Access**
- Install **PuTTY** on **Windows VM (192.168.1.53)**.
- Attempt multiple failed SSH logins to **Kali VM (192.168.1.54)**.

Run:
```bash
ssh kali@192.168.1.54 -p 22
# Enter incorrect password multiple times
```

Verify logs are captured:
```bash
sudo tail -n 20 /var/log/auth.log
```

---
## **3️⃣ Setup Splunk Dashboard & Alerting**

### **Dashboard for Failed Logins (>5 Attempts)**
1. **Go to Splunk Web UI** → **Create Dashboard**.
2. **Add a New Panel** → Run query:
```spl
index=* source="/var/log/auth.log" "Failed password"
| stats count by Source_IP
| where count > 5
```
3. Set **Visualization Type** → Bar Chart.
4. Save the Dashboard.

### **Trigger Alert for Brute Force Attacks**
1. **Save the same search query as an Alert**.
2. **Trigger Condition** → More than **5 failed login attempts in 10 minutes**.
3. **Trigger Actions** → Send an **Email Notification**.

---
## ✅ **Summary**
- Installed and configured **Splunk Forwarder** on **Kali VM (192.168.1.54)**.
- Monitored **SSH failed logins** and forwarded logs to Splunk Server.
- Simulated **brute force attacks** from **Windows VM (192.168.1.53)** using **PuTTY**.
- Created **Splunk Dashboard** and **alert system** for SSH brute-force detection.

🔹 **GitHub Repository for Further Reference.** ✨

---
### 🔗 **Author**: [Your Name]
📅 **Last Updated:** $(date +'%Y-%m-%d')

