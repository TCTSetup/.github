# Organization Setup Guide
This guide explains how to set up a development environment from a freshly formatted computer, including setting up Git, SSH keys, Docker, and SSH tunneling for remote access.

The content of this organization is currently Private. 

If you want to access the repositories, please contact fabio.davolio.1997@gmail.com

---

## Table of Contents
1. [User Setup](#user-setup)
2. [Software Installation](#software-installation)
3. [Virtual Environment Setup](#virtual-environment-setup)
4. [Docker Setup](#docker-setup)
5. [Jupyter Notebook](#jupyter-notebook)
6. [SSH Key Setup](#ssh-key-setup)
7. [SSH Tunneling](#ssh-tunneling)
8. [Troubleshooting](#troubleshooting)

---

## User Setup

### Add User to Sudoers List

Log in as `root` and add the current user to the sudoers list:
```bash
su
sudo adduser <username> sudo
sudo reboot
```

---

## Software Installation
Install the necessary software:
- **Pip**, **Git** and **Vim**:
  
  Run the following commands to install Pip, Git, Vim, and Python virtual environment tools:
  ```bash
  sudo apt-get install python3-pip git-all vim python3.11-venv screen -y
  ```

---

## Virtual Environment Setup
- **Create and Activate a Virtual Environment**:

  After Python is installed, create a virtual environment
  ```bash
  python3 -m venv <name>
  source <name>/bin/activate
  ```
- **Deactivate**

  To deactivate the virtual environment later, simply run:
  ```bash
  deactivate
  ```

---

## Docker Setup

1. **Install Docker and Docker Compose**:

   Follow the [Docker installation guide for Debian](https://docs.docker.com/engine/install/debian/#install-using-the-repository) to install Docker Compose using the apt repository.

2. **Run Docker without `sudo`**:

   Add your user to the Docker group to run Docker without needing `sudo`:
   ```bash
   sudo groupadd docker
   sudo gpasswd -a $USER docker
   sudo reboot
   ```

---
## Jupyter Notebook
1. **Install Jupyter Notebook**
   ```bash
   pip install jupyter
   ```
2. **Run jupyter**
   ```bash
   jupyter notebook --no-browser
   ```
---

## SSH Key Setup
1. **Check if an SSH Key already exists**:

   Check if you already have and SSH key with the following command:
   ```bash
   ssh-add -l -E sha256
   ```
2. **Generate a New SSH Key** (if needed):

   If no key exists, generate a new one:
   ```bash
   ssh-keygen -t ed25519 -C "github_email@example.com"
   ```
   The passphrase will be asked whenever it is required to access the key. It is optional.

3. **Add the SSH Key to GitHub**:
   - Open GitHub, click on your profile picture, and go to **Settings**.
   - In the **Access** section on the left, click **SSH and GPG keys** and then **New SSH Key**.
   - Title the key and copy the contents of `/home/<user>/.ssh/id_ed25519.pub` into the **Key** field.

---

## SSH Tunneling
### On the Host Computer (the one you want to access)
1. **Install OpenSSH Server**:

   To enable SSH access to your host computer:
   ```bash
   sudo apt-get install openssh-server -y
   ```
2. **Configure Router for External Access**:
   - Open your router's settings by typing `192.168.1.1` in your browser.
   - Log in using the router's username and password.
   - Save the device under **Network/LAN/Address Reservation**.
   - Assign a unique external port in **Transmission/NAT/Virtual Servers**.

### On the Client Computer (the one from you access)
3. **Configure SSH for Remote Connections**:

   Set up passwordless SSH by adding the public key from the connecting machine into the `authorized_keys` file of the machine you are accessing:
   ```bash
   cat ~/.ssh/id_rsa.pub | ssh <remote_user>@<remote_host> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
   ```
   `remote_user` and `remote_host` refer to the Host computer.

4. **SSH Config File Setup**:

   If you are frequently connecting from a specific machine, create an SSH config file on the connecting computer (`~/.ssh/config`):
   ```text
   Host alias
     Hostname 172.16.255.255
     Port 25
     User name
     LocalForward 8000 192.168.1.255:8000
   ```
   - **Host**: The alis to use for SSH connection (e.g. `ssh alias`).
   - **Hostname**: The IP of the remote router (change 255 with the actual IP).
   - **Port**: The router's port.
   - **User**: The username of the remote machine.
   - **LocalForward**: Maps a remote machine's port to the local machine allowing access to services.

5. **Fix SSH Known Hosts Errors**:

   If you get connection errors, open the `~/.ssh/known_hosts` file and remove any lines related to the problematic host machine. Then reconnect with `ssh alias`.

---

## Troubleshooting
- **SSH Tunnel Not Working**:

  Check the **SSH Config** file and ensure the correct IP and port numbers are used.

---
