# 🖥️ راهنمای VPS لینوکسی و راه‌اندازی سرور ماینکرافت  
**📘 راهنمای مدیریت VPS لینوکسی (Ubuntu/Debian) و راه‌اندازی سرور ماینکرافت**

---

## 🔑 1. اتصال به VPS  

برای اتصال به VPS لینوکسی (Ubuntu/Debian) از طریق SSH:  
```bash
ssh username@your_vps_ip
```  
`username` را با نام کاربری VPS و `your_vps_ip` را با آدرس IP سرور جایگزین کنید.

---

## ⚙️ 2. دستورات پایه لینوکس  

| 🖥️ دستور | 📘 توضیحات |
|------------|---------------------|
| `ls -la`   | نمایش فایل‌ها و دایرکتوری‌ها (شامل فایل‌های مخفی) |
| `pwd`      | نمایش مسیر فعلی |
| `df -h`    | نمایش فضای دیسک به‌صورت خوانا |
| `free -m`  | نمایش وضعیت RAM به مگابایت |
| `top` یا `htop` | نظارت بر پروسه‌های در حال اجرا |
| `adduser NAME`  | ایجاد کاربر جدید |
| `usermod -aG sudo NAME` | اعطای دسترسی sudo به کاربر |

---

## ☕ 3. نصب جاوا  

ماینکرافت به جاوا نیاز دارد. نسخه پیشنهادی: **Java 17**  
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jre -y
java -version
```  
این دستورات سیستم را به‌روزرسانی کرده و جاوا 17 را نصب می‌کند. نصب را با `java -version` بررسی کنید.

---

## 👤 4. ایجاد کاربر اختصاصی ماینکرافت  

```bash
sudo adduser mcserver
sudo su - mcserver
```  
این دستورات کاربر `mcserver` را برای اجرای سرور ماینکرافت ایجاد کرده و به آن کاربر سوئیچ می‌کند.

---

## 🎮 5. دانلود و اجرای سرور ماینکرافت  

```bash
mkdir ~/minecraft && cd ~/minecraft
wget https://launcher.mojang.com/v1/objects/.../server.jar
java -Xmx2G -Xms1G -jar server.jar nogui
```  
- پس از اجرا، فایل `eula.txt` ایجاد می‌شود. آن را ویرایش کنید و `eula=false` را به `eula=true` تغییر دهید:  
  ```bash
  nano eula.txt
  ```
- ذخیره و خروج: `Ctrl + X`، سپس `Y`، سپس `Enter`.

**توجه:** `...` در URL دستور `wget` را با لینک جدیدترین فایل JAR سرور ماینکرافت از [سایت رسمی Mojang](https://www.minecraft.net/en-us/download/server) جایگزین کنید.

---

## 🔓 6. باز کردن پورت ماینکرافت  

```bash
sudo ufw allow 25565
sudo ufw reload
```  
این دستورات پورت 25565 (پورت پیش‌فرض ماینکرافت) را باز کرده و فایروال را به‌روزرسانی می‌کند.

---

## ♻️ 7. اجرای دائمی سرور  

### روش ۱ (screen / ساده)  
از `screen` برای اجرای سرور در یک جلسه قابل جدا شدن استفاده کنید:  
```bash
screen -S minecraft
java -Xmx2G -Xms1G -jar server.jar nogui
```  
- جدا شدن از جلسه: `Ctrl + A`، سپس `D`.  
- بازگشت به جلسه: `screen -r minecraft`.

### روش ۲ (systemd / پیشرفته)  
یک سرویس systemd برای مدیریت خودکار سرور ایجاد کنید:  
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

سرویس را فعال کنید:  
```bash
sudo systemctl daemon-reload
sudo systemctl enable minecraft
sudo systemctl start minecraft
```  
وضعیت سرویس را بررسی کنید: `sudo systemctl status minecraft`.

---

## 🛠️ 8. مشکلات رایج و راه‌حل‌ها  

| ⚠️ مشکل | 💡 راه‌حل |
|---------------|------------------|
| پورت قبلاً استفاده شده است | پروسه را پیدا و متوقف کنید: `lsof -i :25565` سپس `kill -9 PID` |
| کمبود RAM | مقدار RAM تخصیص‌یافته را افزایش دهید (مثلاً `-Xmx4G` برای 4 گیگابایت) |
| کرش سرور | لاگ‌ها را در `~/minecraft/logs/latest.log` بررسی کنید |

---

## 🔧 اپلیکیشن‌ها و ابزارهای مفید  

### 🖥️ ابزارهای مدیریت SSH و VPS  
- **PuTTY** (ویندوز): یک کلاینت SSH سبک برای اتصال به VPS.  
  - دانلود: [سایت رسمی PuTTY](https://www.putty.org/)  
  - استفاده: آدرس IP سرور، نام کاربری را وارد کرده و متصل شوید. جلسات را برای دسترسی سریع ذخیره کنید.  
- **MobaXterm** (ویندوز): ترمینال همه‌کاره با پشتیبانی از SSH، SFTP و X11 forwarding.  
  - دانلود: [MobaXterm](https://mobaxterm.mobatek.net/)  
  - ویژگی‌ها: جلسات تب‌دار، انتقال فایل و ابزارهای داخلی مانند نمایشگر `top`.  
- **Termius** (ویندوز، مک، لینوکس، iOS، اندروید): کلاینت SSH مدرن با رابط کاربری جذاب و همگام‌سازی ابری.  
  - دانلود: [Termius](https://termius.com/)  
  - استفاده: مناسب برای مدیریت چندین اتصال VPS در دستگاه‌های مختلف.  
- **OpenSSH** (لینوکس/مک): کلاینت SSH داخلی برای لینوکس و مک. از دستور `ssh` مستقیماً در ترمینال استفاده کنید.  
- **FileZilla** (چندپلتفرمی): برای انتقال فایل به/از VPS از طریق SFTP.  
  - دانلود: [FileZilla](https://filezilla-project.org/)  
  - استفاده: با استفاده از IP سرور، نام کاربری و پورت 22 متصل شوید.

### 🖥️ ابزارهای مدیریت VPS  
- **htop**: نمایشگر تعاملی پروسه‌ها برای نظارت بر منابع.  
  ```bash
  sudo apt install htop -y
  ```
- **ncdu**: تحلیلگر تعاملی فضای دیسک.  
  ```bash
  sudo apt install ncdu -y
  ```
- **tmux**: جایگزین `screen` برای جلسات ترمینال پایدار.  
  ```bash
  sudo apt install tmux -y
  ```

### 🎮 ابزارهای مخصوص ماینکرافت  
- **[PaperMC](https://papermc.io/)**: نرم‌افزار سرور بهینه‌شده برای عملکرد بهتر.  
- **[SpigotMC](https://www.spigotmc.org/)**: برای پلاگین‌ها و شخصی‌سازی سرور.  
- **[GeyserMC](https://geysermc.org/)**: امکان اتصال بازیکنان نسخه Bedrock به سرور Java.  
- **[LuckPerms](https://luckperms.net/)**: مدیریت دسترسی‌ها و نقش‌ها در سرور.

### 🔒 امنیت و مانیتورینگ  
- **ufw**: فایروال ساده برای Ubuntu/Debian.  
  ```bash
  sudo apt install ufw -y
  ```
- **fail2ban**: محافظت در برابر حملات brute-force روی SSH.  
  ```bash
  sudo apt install fail2ban -y
  ```
- **netdata**: مانیتورینگ منابع VPS در مرورگر به‌صورت بلادرنگ.  
  - وب‌سایت: [Netdata](https://www.netdata.cloud/)  

---

## 📌 منابع مفید  

- 🌍 [Minecraft Wiki](https://minecraft.wiki)  
- ⚡ [PaperMC](https://papermc.io/)  
- 🔧 [SpigotMC](https://www.spigotmc.org/)
