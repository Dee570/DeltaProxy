# Delta Proxy 🚀

**Delta Proxy** is a powerful and flexible solution for creating your own 4G proxy farm on Linux. The software allows you to manage USB modems, configure proxies (HTTP/SOCKS5), monitor their status through a user-friendly web interface, and automate tasks like IP rotation and connection checks.

🌟 **Project Goal**: Simplify the setup and management of 4G proxies for users, ensuring stability, security, and full control over network connections.

---

## 📋 Table of Contents

1. About the Project
   - Features
   - Concept
   - Architecture
2. Installation
   - Initial Setup
   - Adding Modems
   - Setting Up systemd Service
3. Usage
4. Requirements
5. License

---

## 📖 About the Project

Delta Proxy enables you to set up a 4G proxy farm on your Linux device (PC, mini-PC, or server) with USB modems connected via a USB hub. The software provides a web interface for monitoring and managing modems, as well as an API for automation.

### 🔧 Features

- 🔄 **IP Rotation**: Manual and automatic IP changes on modems.
- 🌐 **Web Interface**: Convenient management of modems, proxy settings, logs, and status.
- 🔌 **Modem Support**: Compatible with most USB modems and LTE modules.
- 🔒 **Authentication**: Secure access to the web interface with login and password.
- 📊 **Monitoring**: Check modem status, IP addresses, and proxy activity.
- ⚙️ **Automation**: Configure routes and connections via the `start_modems.sh` script.
- 📡 **Proxies**: Support for HTTP and SOCKS5 proxies with customizable ports and users.
- 📜 **Logging**: Detailed logs in `/var/log/proxy_farm.log` for diagnostics.

### 📌 Concept

- **Modem**: A device connecting to a mobile network (USB modem or LTE module).
- **Proxy**: An HTTP/SOCKS5 proxy tied to a modem, with a unique port and settings.
- **Web Interface**: The central hub for configuration and monitoring.

### 🏗 Architecture

- **Onsite**: A Linux device (Ubuntu) with a USB hub and modems.
- **Optional**: A VPS for exposing proxy ports to the internet (via `autossh` and `3proxy`).

---

## 🔧 Installation

### 📦 Initial Setup

Delta Proxy runs on Ubuntu (recommended versions: 22.04 or 24.04). Follow these steps to install the software:

1. **Install Ubuntu**:

   - Download the image from the official website (Server or Desktop).
   - Install it on a PC, mini-PC, or laptop.

2. **Download the Binary**:

   - Get the latest `proxy_farm` binary from releases.

3. **Make the Binary Executable**:

   ```bash
   sudo chmod +x /path/to/proxy_farm
   ```

4. **Run the Program**:

   ```bash
   sudo /path/to/proxy_farm --service
   ```

5. **Access the Web Interface**:

   - Open `http://localhost:5000` or `http://<your-box-ip>:5000`.
   - Default login: `admin_delta`, password: `delta_admin`.

> **Note**: Disconnect all USB modems before the first run to avoid conflicts.

### 📡 Adding Modems

1. **Connect Modems**:

   - Plug USB modems into the hub or device ports.

2. **Wait for Initialization**:

   - Allow \~20 seconds for the `start_modems.sh` script to detect and configure modems.

3. **Check the Web Interface**:

   - Navigate to `http://localhost:5000`.
   - Verify that modems appear in the table on the main page.

### 🛠 Setting Up systemd Service

To run Delta Proxy automatically after reboots and in the background, set up a systemd service:

1. **Create the Service File**:

   ```bash
   sudo nano /etc/systemd/system/delta-proxy.service
   ```

2. **Add the Following Content**:

   ```ini
   [Unit]
   Description=Delta Proxy Service
   After=network.target
   
   [Service]
   ExecStart=/usr/local/bin/proxy_farm --service
   Restart=always
   RestartSec=10
   User=root
   StandardOutput=append:/var/log/proxy_farm.log
   StandardError=append:/var/log/proxy_farm.log
   
   [Install]
   WantedBy=multi-user.target
   ```

3. **Enable and Start the Service**:

   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable delta-proxy.service
   sudo systemctl start delta-proxy.service
   ```

4. **Check the Status**:

   ```bash
   sudo systemctl status delta-proxy.service
   ```

> **Logs**: All service messages are written to `/var/log/proxy_farm.log`. View them with:
>
> ```bash
> tail -f /var/log/proxy_farm.log
> ```

---

## 🚀 Usage

Once Delta Proxy is installed and running:

1. **Log in to the Web Interface**:

   - URL: `http://<your-box-ip>:5000`.
   - Login: `admin_delta`, password: `delta_admin` (can be changed in `/settings`).

2. **Manage Modems**:

   - View modem status (IP, state, proxies).
   - Reboot modems or rotate IPs using interface buttons.

3. **Configure Proxies**:

   - In the `/settings` section, set HTTP/SOCKS5 ports, username, and password.
   - Configure VPS forwarding if needed.

4. **Check Logs**:

   - Logs are stored in `/var/log/proxy_farm.log` for troubleshooting.

> **Tip**: If modems don’t appear, verify their connection (`lsusb`) and drivers (`dmesg | grep usb`).

---

## 📋 Requirements

| Component | Requirement |
| --- | --- |
| **OS** | Ubuntu 22.04 or 24.04 (Server/Desktop) |
| **Architecture** | x86_64 |
| **Hardware** | PC/mini-PC with USB ports, USB hub |
| **Modems** | USB modems or LTE modules |
| **Internet** | For checking updates (optional) |
| **Ports** | 5000 (web interface), 8040/9040 (proxies) |

**Dependencies** (installed automatically):

- `autossh`
- `speedtest-cli`
- `3proxy` (version 0.9.3)

---

## 📜 License

Delta Proxy is distributed under the MIT License. Feel free to use, modify, and share the project!

---

**Delta Proxy** is your reliable tool for building 4G proxies! If you have questions or ideas, create an issue or join the discussions. 🚀