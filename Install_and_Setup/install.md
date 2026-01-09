# Netdata Installation Guide

## Important: Do NOT Use Package Managers

**Do not install Netdata using `apt`, `yum`, or your operating system's package repository.** These versions are often outdated and may not include the latest features and security updates.

Instead, use the official Netdata kickstart script as described below.

---

## Installation Steps

### 1. Download and Install Netdata

Run the following command to download and install Netdata using the official kickstart script:

```bash
wget -O /tmp/netdata-kickstart.sh https://get.netdata.cloud/kickstart.sh && sh /tmp/netdata-kickstart.sh
```

This command will:
- Download the latest Netdata installation script
- Automatically install Netdata with all dependencies
- Configure and start the Netdata service

### 2. Verify Installation

Once the installation completes, Netdata should be running automatically.

#### Check Service Status

```bash
sudo systemctl status netdata
```

#### Access the Dashboard

- **Local machine install:** `http://localhost:19999`
- **Remote server / cloud VM install:** `http://your-server-ip:19999`

If the dashboard loads, Netdata has been installed successfully.

---

## Cloud VM Configuration

If you're installing Netdata on a cloud virtual machine (AWS, GCP, Azure, etc.), you need to configure network access.

### Configure Security Group / Firewall Rules

#### 1. Add Inbound Rule for Port 19999

In your cloud provider's security group settings, add an inbound rule:

- **Port**: 19999
- **Protocol**: TCP
- **Source**: Your IP address or appropriate CIDR range (e.g., `0.0.0.0/0` for public access, though this is not recommended for production)

**For production or academic projects, restrict access to your own IP instead of `0.0.0.0/0`.**

#### 2. Ensure HTTP Traffic is Allowed

Make sure HTTP traffic is permitted in your security group inbound rules:

- **Port**: 80 (if you plan to use a reverse proxy)
- **Protocol**: TCP

### Configure Local Firewall (if enabled)

If your server has a firewall enabled (like `ufw` or `firewalld`), you need to open port 19999:

#### For UFW (Ubuntu/Debian):
```bash
sudo ufw allow 19999/tcp
sudo ufw reload
```

#### For firewalld (CentOS/RHEL/Fedora):
```bash
sudo firewall-cmd --permanent --add-port=19999/tcp
sudo firewall-cmd --reload
```

---

## Accessing Netdata

After completing the installation and configuring network access, open your web browser and navigate to:

```
http://your-server-ip:19999
```

You should see the Netdata dashboard with real-time monitoring metrics.

---

## Security Recommendations

- **Restrict Access**: Only allow access to port 19999 from trusted IP addresses
- **Use HTTPS**: Consider setting up a reverse proxy with SSL/TLS encryption
- **Authentication**: Configure access control to protect your monitoring data

---

## Troubleshooting

### Netdata is not accessible
1. Check if Netdata is running: `sudo systemctl status netdata`
2. Verify port 19999 is listening: `sudo netstat -tlnp | grep 19999`
3. Confirm security group/firewall rules are correctly configured
4. Check server logs: `sudo journalctl -u netdata -f`

### Need to restart Netdata
```bash
sudo systemctl restart netdata
```

---

## Additional Resources

- Official Documentation: https://learn.netdata.cloud/
- GitHub Repository: https://github.com/netdata/netdata