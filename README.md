# Firewall Configuration and Testing

## Overview
This project demonstrates the configuration and testing of a Linux firewall using `iptables` and `ufw`. Traffic filtering is performed based on IP addresses and ports. Both configuration methods are explored, and the results of testing are documented.

---

## Tools Used
- **iptables**: A command-line tool for configuring Linux kernel firewall rules.
- **ufw**: An easier-to-use frontend for managing firewall rules.

---

## IP Configuration
- **Kali Linux**: `192.168.56.101`
- **Ubuntu**: `192.168.56.103`

---

## Firewall Configuration Steps

### **Option 1: Using `iptables`**

1. **Configure Rules**:
   ```bash
   # Allow Traffic from Ubuntu (192.168.56.103)
   sudo iptables -A INPUT -s 192.168.56.103 -j ACCEPT

   # Block Traffic from Ubuntu (192.168.56.103)
   sudo iptables -A INPUT -s 192.168.56.103 -j DROP

   # Allow SSH (port 22) from All IPs
   sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

   # Allow HTTP (port 80) Only from Ubuntu
   sudo iptables -A INPUT -s 192.168.56.103 -p tcp --dport 80 -j ACCEPT

   # Block HTTPS (port 443) from All IPs
   sudo iptables -A INPUT -p tcp --dport 443 -j DROP
Save and Restore Rules:

# Save the rules
sudo iptables-save > /etc/iptables/rules.v4

# Restore the rules
sudo iptables-restore < /etc/iptables/rules.v4

## Option 2: Using ufw
# Configure Rules:

# Allow Traffic from Ubuntu (192.168.56.103)
sudo ufw allow from 192.168.56.103

# Block Traffic from Ubuntu (192.168.56.103)
sudo ufw deny from 192.168.56.103

# Allow SSH (port 22) from All IPs
sudo ufw allow ssh

# Allow HTTP (port 80) Only from Ubuntu
sudo ufw allow proto tcp from 192.168.56.103 to any port 80

# Block HTTPS (port 443) from All IPs
sudo ufw deny https

# Enable UFW
sudo ufw enable

# Check active rules
sudo ufw status numbered

## Testing the Firewall

# Ping Test:
From Ubuntu (192.168.56.103), ping the Kali machine (192.168.56.101)
ping 192.168.56.

# Expected Results:
If traffic is blocked, the ping will fail.
If allowed, the ping will succeed.

# HTTP Test:
Start a simple HTTP server on Kali
sudo python3 -m http.server 80

# From Ubuntu, access the HTTP server
curl http://192.168.56.101

# Expected Results:
If HTTP traffic is allowed, the page content will be returned.
If blocked, the connection will fail.

# SSH Test:
From Ubuntu, try SSH into the Kali machine
ssh kali@192.168.56.

# Expected Results:
If SSH traffic is allowed, you can log in.
If blocked, the connection will fail.

## Observations

# Ping Test:
Successfully blocked traffic from 192.168.56.103.

# HTTP Test:
Allowed traffic to port 80 only from 192.168.56.103.

# SSH Test:
Allowed traffic to port 22 from all IPs.

# HTTPS Test:
Successfully blocked HTTPS traffic on port 443.

## Conclusion
This project demonstrates the importance of configuring firewalls for securing Linux systems. By using tools like iptables and ufw, it is possible to control traffic at a granular level, ensuring only authorized access is allowed.