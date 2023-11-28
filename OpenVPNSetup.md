# OpenVPN Server Setup Guide

This guide outlines the steps to install and configure an OpenVPN server on an Ubuntu system.

## Update System

First, update your package lists and upgrade the system:

```bash
sudo apt update
sudo apt upgrade
```

## Install OpenVPN and Easy-RSA

Install OpenVPN and Easy-RSA, which is used to manage SSL certificates:

```bash
sudo apt install openvpn
sudo apt install easy-rsa
```

## Initialize Easy-RSA and Generate Certificates

Initialize the Easy-RSA environment and generate the necessary certificates:

```bash
sudo su
cd /usr/share/easy-rsa
./easyrsa init-pki
./easyrsa build-ca
./easyrsa gen-dh
./easyrsa build-server-full server nopass
exit
```

## Server Configuration

Create and edit the OpenVPN server configuration file:

```bash
sudo gunzip -c /usr/share/doc/openvpn/examples/sample-config-files/server.conf.gz | sudo tee /etc/openvpn/server.conf
sudo nano /etc/openvpn/server.conf
```

Ensure the following lines are uncommented:

```bash
port 1194
proto udp
dev tun
ca /etc/openvpn/easy-rsa/pki/ca.crt
cert /etc/openvpn/easy-rsa/pki/issued/server.crt
key /etc/openvpn/easy-rsa/pki/private/server.key
dh /etc/openvpn/easy-rsa/pki/dh.pem
server 10.8.0.0 255.255.255.0
push "redirect-gateway def1 bypass-dhcp"
push "dhcp-option DNS 8.8.8.8"
push "dhcp-option DNS 8.8.4.4"
user nobody
group nogroup
persist-key
persist-tun
status /var/log/openvpn/openvpn-status.log
verb 3
```

## Configure IP Forwarding

Enable IP forwarding in sysctl:

```bash
sudo nano /etc/sysctl.conf
```

Uncomment the line `net.ipv4.ip_forward=1` and apply changes:

```bash
sudo sysctl -p
```

## Start OpenVPN Service

Enable and start the OpenVPN service:

```bash
sudo systemctl enable openvpn
sudo systemctl start openvpn
```

## Firewall Configuration

Set up the firewall rules, assuming UFW is used:

```bash
sudo ufw allow 1194/udp
sudo ufw allow OpenSSH
sudo ufw enable
```

## Client Configuration

Transfer the client files (`client1.crt`, `client1.key`, `ca.crt`, and `client.ovpn`) to your client devices from `/etc/openvpn/easy-rsa/pki`.

## Client Setup

Install the OpenVPN client software on your client device and import the `client.ovpn` configuration file. Connect to your OpenVPN server using the client software.

## Security Notice

Ensure to secure your keys and configurations and customize them further based on your specific requirements and network setup.
