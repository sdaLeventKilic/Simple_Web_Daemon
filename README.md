# Simple HTTP Server with systemd
This repository demonstrates how to deploy and manage a Python HTTP server as a **systemd**
service on a Linux system.
## Features
- Runs a Python HTTP server on port 8080.
- Automatically starts on system boot.
- Logs output to a file (`/var/log/simple_http_server.log`).
- Restarts automatically if it crashes.
---
## Application Code
The server is implemented in Python and serves files from the working directory.
### **`simple_http_server.py`**
```python
from http.server import SimpleHTTPRequestHandler
from socketserver import TCPServer
PORT = 8080
class CustomHandler(SimpleHTTPRequestHandler):
def log_message(self, format, *args):
with open("/var/log/simple_http_server.log", "a") as log_file:
log_file.write("%s - - [%s] %s\n" % (self.address_string(), self.log_date_time_string(), format %
args))
with TCPServer(("", PORT), CustomHandler) as httpd:
print(f"Serving on port {PORT}")
httpd.serve_forever()
```
---
## Deployment Instructions
### 1. **Create the Application Directory**
```bash
sudo mkdir -p /opt/simple_http_server
```
### 2. **Place the Script in the Directory**
Save the `simple_http_server.py` script in `/opt/simple_http_server/`.
### 3. **Set Permissions**
```bash
sudo chown -R root:nogroup /opt/simple_http_server
sudo chmod -R 755 /opt/simple_http_server
sudo mkdir -p /var/log/
sudo touch /var/log/simple_http_server.log
sudo chmod 664 /var/log/simple_http_server.log
sudo chown nobody:nogroup /var/log/simple_http_server.log
```
### 4. **Create the `systemd` Unit File**
#### **Unit File: `/etc/systemd/system/simple_http_server.service`**
```ini
[Unit]
Description=Simple HTTP Server
After=network.target
[Service]
ExecStart=/usr/bin/python3 /opt/simple_http_server/simple_http_server.py
WorkingDirectory=/opt/simple_http_server
Restart=always
User=nobody
Group=nogroup
StandardOutput=append:/var/log/simple_http_server.log
StandardError=append:/var/log/simple_http_server.log
[Install]
WantedBy=multi-user.target
```
Copy this file to `/etc/systemd/system/`.
### 5. **Reload `systemd` Configuration**
```bash
sudo systemctl daemon-reload
```
### 6. **Enable the Service**
Set the service to start on boot:
```bash
sudo systemctl enable simple_http_server
```
### 7. **Start the Service**
```bash
sudo systemctl start simple_http_server
```
---
## Verification
### Check Service Status
```bash
sudo systemctl status simple_http_server
```
### View Logs
```bash
tail -f /var/log/simple_http_server.log
```
### Test the HTTP Server
Open a browser or use `curl` to test:
```bash
curl http://localhost:8080
```
---
## Summary
### File Locations
1. **Application Script**: `/opt/simple_http_server/simple_http_server.py`
2. **Systemd Unit File**: `/etc/systemd/system/simple_http_server.service`
3. **Log File**: `/var/log/simple_http_server.log`
This setup ensures the server runs automatically on boot, restarts on failure, and logs output to a file.