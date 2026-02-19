# WEB STACK IMPLEMENTATION (LAMP STACK) IN AWS – Project 107

## 📌 Project Overview

This project demonstrates how to deploy a **LAMP Stack (Linux, Apache, MySQL, PHP)** on an AWS EC2 Ubuntu Server.

### What is LAMP?

LAMP is a web development stack consisting of:

- **Linux** – Operating System
- **Apache** – Web Server
- **MySQL** – Database Management System
- **PHP** – Server-side scripting language

---

## ☁️ AWS Environment Setup

### 1️⃣ Create AWS Account
- Register for an AWS account.
- Select your preferred region.
- Launch an EC2 instance:
  - Instance Type: `t2.micro`
  - OS: **Ubuntu Server 24.04 LTS (HVM)**
- Download and securely save your `.pem` private key.

⚠️ IMPORTANT:
- Never share your `.pem` file.
- If lost, you cannot access your server again.
- Stop your EC2 instance when not in use to avoid exceeding free tier limits.

---

## 🔐 Connect to EC2 Instance

### For Windows (Using SSH)

```bash
cd Downloads
ssh -i <private-key-name>.pem ubuntu@<Public-IP-address>
```

# 🚀 Step 1 — Install Apache and Configure Firewall

## Update Packages

```bash
sudo apt update
```

## Install Apache

```bash
sudo apt install apache2
```

## Verify Apache Status

```bash
sudo systemctl status apache2
```

## Test Locally

```bash
curl http://localhost:80
curl http://127.0.0.1:80
```

## Open Port 80 in AWS

- Go to EC2 → Security Groups
- Add inbound rule:
  - Type: HTTP
  - Port: 80
  - Source: 0.0.0.0/0

## Test in Browser

```
http://<Public-IP-Address>:80
```

---

# 🛢 Step 2 — Install MySQL

## Install MySQL Server

```bash
sudo apt install mysql-server
```

## Access MySQL

```bash
sudo mysql
```

## Set Root Password

```sql
ALTER USER 'root'@'localhost'
IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
```

Exit MySQL:

```sql
exit;
```

## Run Security Script

```bash
sudo mysql_secure_installation
```

Follow prompts and secure your installation.

## Test Login

```bash
sudo mysql -p
```

---

# 🐘 Step 3 — Install PHP

## Install PHP and Required Modules

```bash
sudo apt install php libapache2-mod-php php-mysql
```

## Verify PHP Version

```bash
php -v
```

---

# 🌐 Step 4 — Configure Apache Virtual Host

## Create Website Directory

```bash
sudo mkdir /var/www/projectlamp
sudo chown -R $USER:$USER /var/www/projectlamp
```

## Create Virtual Host Config File

```bash
sudo vi /etc/apache2/sites-available/projectlamp.conf
```

### Add Configuration:

```apache
<VirtualHost *:80>
    ServerName projectlamp
    ServerAlias www.projectlamp
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/projectlamp
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

## Enable Site

```bash
sudo a2ensite projectlamp
sudo a2dissite 000-default
sudo apache2ctl configtest
sudo systemctl reload apache2
```

## Create Test Page

```bash
echo "Hello LAMP from AWS" > /var/www/projectlamp/index.html
```

Test in browser:

```
http://<Public-IP-Address>
```

---

# 🔄 Step 5 — Enable PHP on Website

## Modify DirectoryIndex Order

```bash
sudo vim /etc/apache2/mods-enabled/dir.conf
```

Change:

```apache
DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
```

To:

```apache
DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
```

Reload Apache:

```bash
sudo systemctl reload apache2
```

---

## Create PHP Test File

```bash
vim /var/www/projectlamp/index.php
```

Add:

```php
<?php
phpinfo();
```

Visit in browser:

```
http://<Public-IP-Address>
```

If PHP info page appears ✅ — your LAMP stack is working!

---

## 🧹 Remove PHP Test File (Security)

```bash
sudo rm /var/www/projectlamp/index.php
```

---

# ✅ Final Architecture

- ☑ Linux (Ubuntu Server)
- ☑ Apache Web Server
- ☑ MySQL Database
- ☑ PHP Processing Engine
- ☑ Hosted on AWS EC2

---

# 🎯 Conclusion

In this project, I successfully:

- Created an AWS EC2 Ubuntu server
- Installed and configured Apache
- Installed and secured MySQL
- Installed PHP
- Configured Apache Virtual Host
- Verified full LAMP functionality

The LAMP stack is now fully operational and ready for hosting dynamic web applications.

---

# 📎 Important Notes
- Always stop EC2 instance when not in use
- Public IP changes after stopping/starting instance
- Keep your `.pem` file secure

---

## 👨‍💻 Author

Marco Raafat Zakaria  
Steghub scolarship

---

