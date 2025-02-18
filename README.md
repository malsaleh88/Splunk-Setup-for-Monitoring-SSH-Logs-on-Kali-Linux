# 🚀 Splunk SSH Brute-Force Detection Setup

## 📌 Overview
This repository documents the setup of **Splunk Forwarder** on **Kali Linux VM (192.168.1.54)** to monitor **SSH login attempts** and forward logs to **Splunk Server**. Additionally, it includes a **dashboard** and an **alert system** to detect multiple failed login attempts.

---
## 🛠️ Setup & Configuration

### **1️⃣ Install and Configure Splunk Forwarder on Kali VM (192.168.1.54)**
```bash
# Download and Install Splunk Forwarder (if not installed)
sudo dpkg -i splunkforwarder-9.4.0-6b4ebe426ca6-linux-amd64.deb
```

#### **Check if Splunk Forwarder is Running**
```bash
sudo /opt/splunkforwarder/bin/splunk status
```

#### **List Monitored Files**
```bash
sudo /opt/splunkforwarder/bin/splunk list monitor
```
![list](https://github.com/user-attachments/assets/ca8b9b2b-2291-4e9d-9fa9-6abe9b2d878f)



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

![new search faild](https://github.com/user-attachments/assets/0326c7ff-b8aa-41fd-8107-887005ee4524)



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
![putty](https://github.com/user-attachments/assets/201fa179-6dbf-4212-b4da-be03d7336a49)

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

![dashboard](https://github.com/user-attachments/assets/6ca22c2b-3247-4816-90bb-fae4f2b3b074)

![dashbourd](https://github.com/user-attachments/assets/f5134859-2e2f-44db-a3fa-2a6e86b9fdd9)

### **Trigger Alert for Brute Force Attacks**
1. **Save the same search query as an Alert**.
2. **Trigger Condition** → More than **5 failed login attempts in 10 minutes**.
3. **Trigger Actions** → Send an **Email Notification**.



![aletr1](https://github.com/user-attachments/assets/9b082605-5842-4882-9e15-6851c168f91c)


![alert2](https://github.com/user-attachments/assets/51b0607f-c9ba-4f2a-b23b-844e0f82d9b6)


## ✅ **Summary**
- Installed and configured **Splunk Forwarder** on **Kali VM (192.168.1.54)**.
- Monitored **SSH failed logins** and forwarded logs to Splunk Server.
- Simulated **brute force attacks** from **Windows VM (192.168.1.53)** using **PuTTY**.
- Created **Splunk Dashboard** and **alert system** for SSH brute-force detection.


