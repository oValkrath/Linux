# 🖥️ Linux VPS Help Desk + Minecraft Server Setup  
**📘 راهنمای مدیریت VPS لینوکسی (Ubuntu/Debian) و راه‌اندازی سرور ماینکرافت**  
*A bilingual guide for managing a Linux VPS (Ubuntu/Debian) and setting up a Minecraft server.*  

---

## 🔑 1. اتصال به VPS | Connect to VPS  

**فارسی:**  
برای اتصال به سرور لینوکسی (Ubuntu/Debian):  
```bash
ssh username@your_vps_ip
```  

**English:**  
To connect to your Linux VPS (Ubuntu/Debian):  
```bash
ssh username@your_vps_ip
```  

---

## ⚙️ 2. دستورات پایه لینوکس | Basic Linux Commands  

| 🖥️ دستور / Command | 📖 توضیح (FA) | 📘 Description (EN) |
|---------------------|---------------|---------------------|
| `ls -la`           | لیست فایل‌ها | List files |
| `pwd`              | مسیر فعلی | Show current path |
| `df -h`            | فضای دیسک | Disk usage |
| `free -m`          | وضعیت RAM | RAM usage |
| `top` یا `htop`    | پروسه‌ها | Processes |
| `adduser NAME`     | ساخت کاربر | Create user |
| `usermod -aG sudo NAME` | دسترسی sudo | Give sudo access |

---

## ☕ 3. نصب Java | Install Java  

**فارسی (Ubuntu/Debian):**  
ماینکرافت نیاز به جاوا دارد. نسخه پیشنهادی: **Java 17**  
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jre -y
java -version
```  

**English (Ubuntu/Debian):**  
Minecraft requires Java. Recommended version: **Java 17**  
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jre -y
java -version
```  

---

## 👤 4. ساخت یوزر مخصوص Minecraft | Create a Dedicated User  

```bash
sudo adduser mcserver
sudo su - mcserver
```  

---

## 🎮 5. دانلود و اجرای Minecraft Server | Download & Run Server  

**فارسی:**  
```bash
mkdir ~/minecraft && cd ~/minecraft
wget https://launcher.mojang.com/v1/objects/.../server.jar
java -Xmx2G -Xms1G -jar server.jar nogui
```
- فایل `eula.txt` ساخته می‌شود → `eula=false` را به `eula=true` تغییر دهید.  

**English:**  
```bash
mkdir ~/minecraft && cd ~/minecraft
wget https://launcher.mojang.com/v1/objects/.../server.jar
java -Xmx2G -Xms1G -jar server.jar nogui
```
- File `eula.txt` will be created → change `eula=false` to `eula=true`.  

---

## 🔓 6. باز کردن پورت | Open Port  

```bash
sudo ufw allow 25565
sudo ufw reload
```  

---

## ♻️ 7. اجرای دائمی سرور | Keep Server Running  

**روش ۱ (screen / روش ساده) | Method 1 (screen / simple):**
```bash
screen -S minecraft
java -Xmx2G -Xms1G -jar server.jar nogui
# جدا شدن: Ctrl + A + D  |  Detach: Ctrl + A + D
```  

**روش ۲ (systemd / روش حرفه‌ای) | Method 2 (systemd / advanced):**
```ini
# /etc/systemd/system/minecraft.service
[Unit]
Description=Minecraft Server
After=network.target

[Service]
User=mcserver
WorkingDirectory=/home/mcserver/minecraft
ExecStart=/usr/bin/java -Xmx2G -Xms1G -jar server.jar nogui
Restart=on-failure

[Install]
WantedBy=multi-user.target
```  

```bash
sudo systemctl daemon-reload
sudo systemctl enable minecraft
sudo systemctl start minecraft
```  

---

## 🛠️ 8. مشکلات رایج | Common Issues  

| ⚠️ مشکل (FA) | 💡 راه‌حل (FA) | ⚠️ Issue (EN) | 💡 Solution (EN) |
|--------------|----------------|---------------|-----------------|
| Port already in use | پیدا کردن و Kill کردن پروسه | Port already in use | Find and kill process: `lsof -i :25565` + `kill -9 PID` |
| RAM کم | تغییر `-Xmx` | Low RAM | Adjust `-Xmx` (e.g., `-Xmx4G`) |
| کرش سرور | بررسی `latest.log` | Server crash | Check `latest.log` |

---

## 🔧 اپلیکیشن‌ها و ابزارهای مفید | Useful Apps & Tools  

### 🖥️ مدیریت VPS
- **htop** → نمایش پروسه‌ها و مصرف منابع  
  ```bash
  sudo apt install htop -y
  ```
- **ncdu** → بررسی حجم دیسک به صورت تعاملی  
  ```bash
  sudo apt install ncdu -y
  ```
- **tmux** یا **screen** → اجرای دائمی برنامه‌ها و سوییچ بین جلسات ترمینال  

---

### 🎮 مخصوص Minecraft
- **[PaperMC](https://papermc.io/)** → نسخه‌ی بهینه شده‌ی ماینکرافت برای پرفورمنس بهتر  
- **[SpigotMC](https://www.spigotmc.org/)** → برای پلاگین‌ها و شخصی‌سازی سرور  
- **[GeyserMC](https://geysermc.org/)** → برای وصل شدن بازیکنان نسخه Bedrock به سرور Java  
- **[LuckPerms](https://luckperms.net/)** → مدیریت دسترسی‌ها و نقش‌ها در سرور  

---

### 🔒 امنیت و مانیتورینگ
- **ufw** → فایروال ساده برای Ubuntu/Debian  
  ```bash
  sudo apt install ufw -y
  ```
- **fail2ban** → جلوگیری از حملات brute-force روی SSH  
  ```bash
  sudo apt install fail2ban -y
  ```
- **netdata** → مانیتورینگ منابع VPS در مرورگر  
  [Netdata Website](https://www.netdata.cloud/)  

---

## 📌 منابع مفید | Useful Resources  

- 🌍 [Minecraft Wiki](https://minecraft.wiki)  
- ⚡ [PaperMC (Performance)](https://papermc.io/)  
- 🔧 [SpigotMC](https://www.spigotmc.org/)  

---
