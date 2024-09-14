# Secure VPS Server

This guide provides step-by-step instructions for setting up a secure VPS server. It covers system updates, SSH configuration, firewall setup, and intrusion prevention.

## 1. Update and Upgrade the System

First, ensure your system is up-to-date:

```bash
sudo apt update && sudo apt upgrade -y
```

This command updates the package lists and upgrades all installed packages to their latest versions.

## 2. Change the Default SSH Listening Port

Changing the default SSH port adds an extra layer of security by making it harder for automated attacks to find your SSH service.

**1. Edit the SSH configuration file:**

```bash
sudo nano /etc/ssh/sshd_config
```

**2. Find and modify the Port line:**

```text
#Port 22
```

Replace it with:

```text
Port 49152
```

Choose a port number between 49152 and 65535 to avoid conflicts with well-known services.

**3. Restart the SSH service:**

```bash
sudo systemctl restart sshd
```

Alternatively, reboot the VPS:

```bash
sudo reboot
```

## 3. Configure a Firewall (UFW)

UFW (Uncomplicated Firewall) provides a user-friendly way to manage your firewall rules.

**1. Install UFW:**

```bash
sudo apt install ufw -y
```

**2. Set default rules:**

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

This configuration denies all incoming connections and allows all outgoing connections by default.

**3. Allow SSH on the custom port:**

```bash
sudo ufw allow 49152/tcp
```

Replace 49152 with your chosen SSH port.

**4. If using Nginx, allow HTTP and HTTPS:**

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

**5. If accessing Strapi directly, allow its port:**

```bash
sudo ufw allow 1337/tcp
```

**6. Enable UFW and check its status:**

```bash
sudo ufw enable
sudo ufw status verbose
```

## 4. Install and Configure Fail2ban

Fail2ban helps protect against brute-force attacks by temporarily banning IP addresses that show malicious signs.

**1. Install Fail2ban:**

```bash
sudo apt install fail2ban -y
```

**2. Configure Fail2ban:**

```bash
sudo nano /etc/fail2ban/jail.local
```

**3. Add or modify the following configuration:**

```ini
[sshd]
enabled = true
port = 49152
maxretry = 3
findtime = 5m
bantime = 30m
```

This configuration:

- Enables Fail2ban for SSH
- Sets it to monitor your custom SSH port
- Bans an IP after 3 failed attempts within 5 minutes
- Sets the ban time to 30 minutes

**4. Restart Fail2ban:**

```bash
sudo systemctl restart fail2ban
```

By following these steps, you'll significantly improve the security of your VPS. Remember to always keep your system updated and monitor for any suspicious activities.
