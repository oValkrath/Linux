# 🖥️ Linux VPS Help Desk + Minecraft Server Setup  
**📘 Guide to Managing a Linux VPS (Ubuntu/Debian) and Setting Up a Minecraft Server**

---

## 🔑 1. Connect to Your VPS  

To connect to your Linux VPS (Ubuntu/Debian) via SSH:  
```bash
ssh username@your_vps_ip
```  
Replace `username` with your VPS user and `your_vps_ip` with the server's IP address.

---

## ⚙️ 2. Basic Linux Commands  

| 🖥️ Command | 📘 Description |
|------------|---------------------|
| `ls -la`   | List files and directories (including hidden) |
| `pwd`      | Display current working directory |
| `df -h`    | Show disk usage in human-readable format |
| `free -m`  | Display RAM usage in megabytes |
| `top` or `htop` | Monitor running processes |
| `adduser NAME`  | Create a new user |
| `usermod -aG sudo NAME` | Grant sudo privileges to a user |

---

## ☕ 3. Install Java  

Minecraft requires Java. Recommended version: **Java 17**  
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install openjdk-17-jre -y
java -version
```  
This updates the system and installs Java 17. Verify the installation with `java -version`.

---

## 👤 4. Create a Dedicated Minecraft User  

```bash
sudo adduser mcserver
sudo su - mcserver
```  
This creates a user `mcserver` for running the Minecraft server and switches to that user.

---

## 🎮 5. Download and Run the Minecraft Server  

```bash
mkdir ~/minecraft && cd ~/minecraft
wget https://launcher.mojang.com/v1/objects/.../server.jar
java -Xmx2G -Xms1G -jar server.jar nogui
```  
- After running, a `eula.txt` file is created. Edit it to change `eula=false` to `eula=true`:  
  ```bash
  nano eula.txt
  ```
- Save and exit (`Ctrl + X`, then `Y`, then `Enter`).

**Note:** Replace the `...` in the `wget` URL with the latest Minecraft server JAR link from [Mojang’s official site](https://www.minecraft.net/en-us/download/server).

---

## 🔓 6. Open the Minecraft Port  

```bash
sudo ufw allow 25565
sudo ufw reload
```  
This opens port 25565 (Minecraft’s default port) and reloads the firewall.

---

## ♻️ 7. Keep the Server Running  

### Method 1 (screen / Simple)  
Use `screen` to run the server in a detachable session:  
```bash
screen -S minecraft
java -Xmx2G -Xms1G -jar server.jar nogui
```  
- Detach from the session: `Ctrl + A`, then `D`.  
- Reattach later: `screen -r minecraft`.

### Method 2 (systemd / Advanced)  
Create a systemd service for automatic server management:  
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

Activate the service:  
```bash
sudo systemctl daemon-reload
sudo systemctl enable minecraft
sudo systemctl start minecraft
```  
Check status: `sudo systemctl status minecraft`.

---

## 🛠️ 8. Common Issues and Solutions  

| ⚠️ Issue | 💡 Solution |
|---------------|------------------|
| Port already in use | Find and terminate the process: `lsof -i :25565` then `kill -9 PID` |
| Low RAM | Increase allocated RAM (e.g., `-Xmx4G` for 4GB) |
| Server crash | Check logs in `~/minecraft/logs/latest.log` for errors |

---

## 🔧 Useful Applications and Tools  

### 🖥️ SSH and VPS Management Tools  
- **PuTTY** (Windows): A lightweight SSH client for connecting to your VPS.  
  - Download: [PuTTY Official Site](https://www.putty.org/)  
  - Usage: Enter your VPS IP, username, and connect. Save sessions for quick access.  
- **MobaXterm** (Windows): An all-in-one terminal with SSH, SFTP, and X11 forwarding.  
  - Download: [MobaXterm](https://mobaxterm.mobatek.net/)  
  - Features: Tabbed sessions, file transfer, and built-in tools like `top` viewer.  
- **Termius** (Windows, macOS, Linux, iOS, Android): A modern SSH client with a sleek interface and cloud sync.  
  - Download: [Termius](https://termius.com/)  
  - Usage: Ideal for managing multiple VPS connections across devices.  
- **OpenSSH** (Linux/macOS): Built-in SSH client for Linux and macOS. Use the `ssh` command directly in the terminal.  
- **FileZilla** (Cross-Platform): For transferring files to/from your VPS via SFTP.  
  - Download: [FileZilla](https://filezilla-project.org/)  
  - Usage: Connect using your VPS IP, username, and port 22 for secure file transfers.  

### 🖥️ VPS Management Tools  
- **htop**: Interactive process viewer for resource monitoring.  
  ```bash
  sudo apt install htop -y
  ```
- **ncdu**: Interactive disk usage analyzer.  
  ```bash
  sudo apt install ncdu -y
  ```
- **tmux**: Alternative to `screen` for persistent terminal sessions.  
  ```bash
  sudo apt install tmux -y
  ```

### 🎮 Minecraft-Specific Tools  
- **[PaperMC](https://papermc.io/)**: Optimized Minecraft server software for better performance.  
- **[SpigotMC](https://www.spigotmc.org/)**: For plugins and server customization.  
- **[GeyserMC](https://geysermc.org/)**: Allows Bedrock edition players to join Java servers.  
- **[LuckPerms](https://luckperms.net/)**: Manages permissions and roles on the server.

### 🔒 Security and Monitoring  
- **ufw**: Simple firewall for Ubuntu/Debian.  
  ```bash
  sudo apt install ufw -y
  ```
- **fail2ban**: Protects against brute-force attacks on SSH.  
  ```bash
  sudo apt install fail2ban -y
  ```
- **netdata**: Real-time VPS resource monitoring in the browser.  
  - Website: [Netdata](https://www.netdata.cloud/)  

---

## 📌 Useful Resources  

- 🌍 [Minecraft Wiki](https://minecraft.wiki)  
- ⚡ [PaperMC (Performance)](https://papermc.io/)  
- 🔧 [SpigotMC](https://www.spigotmc.org/)
