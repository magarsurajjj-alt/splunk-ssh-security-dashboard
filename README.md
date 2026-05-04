# 🔐 Splunk SSH Security Dashboard

![Splunk](https://img.shields.io/badge/Splunk-Dashboard-green)
![Status](https://img.shields.io/badge/Status-Active-brightgreen)

## 📌 Overview
This project provides a **Splunk dashboard** for monitoring SSH authentication logs. It helps identify suspicious activities such as failed login attempts, brute force attacks, and unauthorized access using visual analytics.

---

## 🎯 Objective
- Monitor SSH authentication events  
- Detect brute-force login attempts  
- Analyze login trends  
- Visualize attacker locations using geo-mapping  

---

## ⚙️ Setup Instructions

### 1. Add Time Range Picker
- Label: `Time Range`  
- Token: `time_range`  

### 2. Add Submit Button
- Used to apply selected filters  

📌 **Note:** Use `time_range` for all panels.

---

## 📊 Dashboard Panels

### 🔹 Authentication Overview

#### Total SSH Events
```spl
source="ssh_logs.json" host="LinuxServer" sourcetype="_json"
| stats count AS "Total SSH Events"

Successful Logins
source="ssh_logs_new.json" host="LinuxServer" sourcetype="_json" event_type="Successful SSH Login"
| stats count AS "Successful Logins"

Failed Logins
source="ssh_logs_new.json" host="LinuxServer" sourcetype="_json" event_type="Failed SSH Login"
| stats count AS "Failed Logins"

Invalid User Attempts
index=auth "sshd" "invalid user"
| stats count AS "Invalid User Attempts"

📈 Login Activity Trends
Failed Logins by Username
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Failed SSH Login"
| top username

Possible Brute Force (Top IPs)
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| top id.orig_h

🌍 Geo-location Analysis
Brute Force Attack Map
source="ssh_logs_new.json" host="LinuxNew" sourcetype="_json" event_type="Multiple Failed Authentication Attempts"
| table id.orig_h
| iplocation id.orig_h
| stats count by Country
| geom geo_countries featureIdField="Country"
```
📂 Project Structure
```
splunk-ssh-security-dashboard/
│── README.md
│── dashboard/
│   └── ssh_dashboard.xml
│── data/
│   ├── ssh_logs.json
│   └── ssh_logs_new.json
│── screenshots/
│   ├── dashboard.png
│   └── readme_preview.png
```
📷 Screenshots
🔹 Dashboard Overview

🚀 Features

✔ Real-time SSH monitoring
✔ Detection of brute-force attacks
✔ Visualization using charts and maps
✔ Geo-location tracking of attackers

🛠️ Requirements
Splunk Enterprise / Splunk Cloud
SSH logs in JSON format
iplocation and geom commands enabled
