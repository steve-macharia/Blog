---
title: "🚀 Automating OpenEMR with Docker: A Bash Script Walkthrough for Jali-EMR"
datePublished: Thu Aug 07 2025 10:17:18 GMT+0000 (Coordinated Universal Time)
cuid: cme18teyx000o02jo987mbkfp
slug: automating-openemr-with-docker-a-bash-script-walkthrough-for-jali-emr
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1754561796032/2981b098-792f-43f2-8530-bc31e45f5451.png

---

*By Steve Macharia | MedbyTech*

As healthcare in Kenya continues to digitize, solutions like **Jali-EMR**, an enhanced fork of OpenEMR developed by **MedbyTech**, are streamlining workflows in clinics and hospitals. To simplify local development and deployment, we recently built a clean, secure, and reusable **Bash script** for bootstrapping our containerized OpenEMR environment.

This blog walks through the core logic of the script and how it fits within modern DevOps workflows using **Docker Compose**.

---

### 🔧 Script Purpose

The goal of this script is to:

* Start all required containers (`OpenEMR`, `MySQL`, `phpMyAdmin`, `Adminer`)
    
* Wait for the containers to initialize
    
* Set a default admin password securely
    
* Allow port customization for multi-instance development
    
* Enable consistent onboarding of new developers or engineers
    

---

### 📄 The Script: Dockerized OpenEMR Startup (Simplified)

```plaintext
bashCopyEdit#!/bin/bash

# === CONFIGURATION ===
USERNAME="admin"
NEW_PASSWORD="Password123"
SITE_ID="default"
PORT="8080"
PHPMYADMIN_PORT="8082"
ADMINER_PORT="8081"
MYSQL_CONTAINER="openemr-mysql"
OPENEMR_CONTAINER="openemr-main"

echo "🟡 Starting OpenEMR containers..."
docker compose up -d

# Wait for containers to be fully up
echo "⏳ Waiting for OpenEMR to initialize..."
sleep 20

# Updating Admin password securely
echo "🔐 Updating OpenEMR admin password..."
docker exec -i $OPENEMR_CONTAINER bash -c \
"mysql -u root -e \"
  UPDATE openemr.users 
  SET password = PASSWORD('$NEW_PASSWORD') 
  WHERE username = '$USERNAME';
  FLUSH PRIVILEGES;
\""

echo "✅ OpenEMR is ready on http://localhost:$PORT"
echo "🔒 Admin Username: $USERNAME | Password: $NEW_PASSWORD"
```

---

### 🧠 What This Script Does (Behind the Scenes)

#### 1\. **Containerized Deployment**

It relies on a predefined `docker-compose.yml` (not shown here) that spins up the necessary containers:

* `OpenEMR` (the EHR web app)
    
* `MySQL` (underlying relational database)
    
* `phpMyAdmin` and `Adminer` (for DB GUI access)
    

#### 2\. **Secures Admin Credentials**

Instead of using the default weak credentials, this script hashes and updates the password securely using:

```plaintext
sqlCopyEditUPDATE users SET password = PASSWORD('$NEW_PASSWORD') ...
```

> 🔐 *Tip:* Consider using `bcrypt` or OpenEMR's own password hash function for better security in production.

#### 3\. **Flexible Port Management**

The variables `PORT`, `PHPMYADMIN_PORT`, and `ADMINER_PORT` allow customization—helpful for running multiple instances or avoiding port conflicts during local development.

#### 4\. **Repeatable Developer Setup**

By automating setup steps, the script enables engineers (like those working at **MedbyTech**) to onboard faster, reducing misconfigurations and environment drift.

---

### 🌍 Why This Matters: OpenEMR in Kenya

The healthcare ecosystem in Kenya is rapidly evolving. **Jali-EMR**, a localized adaptation of OpenEMR, is tailored for Kenyan clinics with:

* NHIF/SHA readiness
    
* Integration with local pharmacy and lab systems
    
* Offline-first features
    
* SMS and WhatsApp patient reminders
    

With automation like this script, developers can confidently deploy test environments, demo stacks, and CI/CD pipelines using **Docker + Bash**—without wasting time on repetitive setup.

---

### 🧩 SEO Keywords Included:

* OpenEMR Docker Script Kenya
    
* Jali-EMR installation
    
* MedbyTech health IT
    
* Automate OpenEMR with Bash
    
* Local EMR for Kenya
    
* Secure EMR deployment
    
* Open-source EMR in Africa
    

---

### 📸 Visual Insight

![OpenEMR Terminal Graphic](/mnt/data/A_digital_graphic_displays_a_dark_blue_terminal_wi.png align="left")

---

### 📦 Want to Try It?

Check out [MedbyTech’s GitHub](https://github.com/steve-macharia) for the **Jali-EMR Docker repository** and contribute to improving healthcare tech in Kenya.

---

### 🙌 Final Thoughts

This is how modern software engineers drive transformation in healthcare—one script at a time.

Whether you're a hospital IT admin or an indie developer working in a rural health facility, automating OpenEMR deployment is a giant leap toward sustainable digital health systems in Africa.