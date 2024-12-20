# Post Server Setup Checklist (Ubuntu 22.04)

This guide helps you configure and secure your Ubuntu 22.04 server after a fresh installation. It excludes tools like Java, Docker, and Vim.

---

## 1. **Update and Upgrade the System**
```bash
sudo apt update && sudo apt upgrade -y
```

---

## 2. **Set the Hostname**
```bash
sudo hostnamectl set-hostname <your-hostname>
```
Verify the hostname:
```bash
hostnamectl
```

---

## 3. **Create a New User (Optional)**
```bash
sudo adduser <username>
```
Grant administrative privileges:
```bash
sudo usermod -aG sudo <username>
```

---

## 4. **Configure the Firewall (UFW)**
Install UFW:
```bash
sudo apt install ufw -y
```
Allow SSH and other essential services:
```bash
sudo ufw allow OpenSSH
sudo ufw allow 5432  # PostgreSQL (if applicable)
```
Enable the firewall:
```bash
sudo ufw enable
sudo ufw status
```

---

## 5. **Secure SSH Access**
Edit the SSH configuration file:
```bash
sudo nano /etc/ssh/sshd_config
```
- Change the SSH port (default is 22):
  ```
  Port 2222
  ```
- Disable root login:
  ```
  PermitRootLogin no
  ```
Restart the SSH service:
```bash
sudo systemctl restart ssh
```

---

## 6. **Install Essential Tools**
```bash
sudo apt install build-essential curl wget git htop net-tools -y
```

---

## 7. **Install Node.js (Latest Version)**
Install Node.js via NodeSource:
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```
Verify installation:
```bash
node -v
npm -v
```

---

## 8. **Install PostgreSQL**
Install PostgreSQL:
```bash
sudo apt install postgresql postgresql-contrib -y
```
Start and enable PostgreSQL:
```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```
Switch to the PostgreSQL user:
```bash
sudo -u postgres psql
```

---

## 9. **Configure Timezone**
Set the correct timezone:
```bash
sudo timedatectl set-timezone <your-timezone>
```
Example:
```bash
sudo timedatectl set-timezone Asia/Jakarta
```
Verify:
```bash
timedatectl
```

---

## 10. **Monitor System Resources**
Install monitoring tools:
```bash
sudo apt install sysstat -y
```
Enable monitoring:
```bash
sudo systemctl enable sysstat
sudo systemctl start sysstat
```

---

## 11. **Install PM2 (Process Manager for Node.js)**
```bash
sudo npm install -g pm2
```
Verify installation:
```bash
pm2 -v
```

---

## 12. **Set Up Swap Memory (Optional)**
Check current swap:
```bash
sudo swapon --show
```
Create a swap file if none exists:
```bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
Make it persistent:
```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

---

## 13. **Reboot the System**
After making the changes, reboot the server:
```bash
sudo reboot
