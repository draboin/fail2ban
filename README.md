#!/bin/bash

# Update the system
apt-get update
apt-get upgrade -y

# Install necessary packages
apt-get install -y fail2ban ufw

# Configure the firewall
ufw default deny incoming
ufw default allow outgoing
ufw allow ssh
ufw enable

# Configure fail2ban
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sed -i 's/bantime = 600/bantime = 3600/' /etc/fail2ban/jail.local
sed -i 's/findtime = 600/findtime = 3600/' /etc/fail2ban/jail.local
systemctl restart fail2ban

# Create a new user and add them to the sudo group
adduser [USERNAME]
usermod -aG sudo [USERNAME]
