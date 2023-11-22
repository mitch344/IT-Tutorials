# Samba Server Setup Guide

This guide provides step-by-step instructions for setting up a Samba server on Ubuntu.

## Steps to Setup Samba

### Step 1: Find IP Address

Display IP address:
```bash
ip a
```

### Step 2: Install Samba

Update the package list and install Samba:

```bash
sudo apt update
sudo apt install samba -y
```

### Step 3: Create a Shared Folder

Create the directory you want to share, for example, "sharedfolder":

```bash
sudo mkdir /sharedfolder
```

### Step 4: Set Permissions

Set the folder permissions to be accessible to everyone:

```bash
sudo chmod -R 777 /sharedfolder
```

### Step 5: Configure Samba

Edit the Samba configuration file `/etc/samba/smb.conf`:

```bash
sudo nano /etc/samba/smb.conf
```

### Step 6: Add Samba Share Configuration

Add the following to the `smb.conf` file:

```plaintext
[sharedfolder]
path = /sharedfolder
read only = no
guest ok = yes
```

### Step 7: Restart Samba

Restart the Samba service:

```bash
sudo systemctl restart smbd
```

### Step 8: Set a New Password for Samba User

Set a password for your Samba user:

```bash
sudo smbpasswd -a your_username
```

### Step 9: Check Samba Status

Check the Samba service status:

```bash
sudo systemctl status smbd
```

## Accessing the Shared Folder

Access the shared folder from other devices with smb://<server_ip>/sharedfolder.
