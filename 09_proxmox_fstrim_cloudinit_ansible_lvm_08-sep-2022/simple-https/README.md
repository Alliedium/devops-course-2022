## Simple HTTPS server creation and management with systemctl ##

### Prerequisites ###

1. Python should to be installed:
```
sudo dnf install python3
```

2. There should be 3 files in simple-https directory:
```
cd ./simple-https
```
```
ls
```
result:
```
example-com.conf  make-cert.sh  simple-server.py
```

### Creation ###

1. Run make-cert.sh first in order to generate certificate.

2. Check if the python script can be executed without errors:
```
cd ./simple-https
python3 simple-server.py
```
 
3. In order to start it from the service it's necessary to create it:
```
cd /etc/systemd/system/
sudo nano simple-https.service
```

The service code is below:
```
[Unit]
Description=Simple HTTPS Server
[Service]
User=tatyana
WorkingDirectory=/home/tatyana/
Type=simple
ExecStart=/usr/bin/python3 /home/tatyana/simple-https/simple-server.py
[Install]
WantedBy=multi-user.target
```

4. Once a new service is added reload the systemctl daemon:
```
sudo systemctl daemon-reload
```

### Management ###

Start the service:
```
sudo systemctl start simple-https.service
```

Check server's response:
```
curl https://10.115.176.36:4443 --insecure
```

Stop the service:
```
sudo systemctl stop simple-https.service
```

Check server's response to make sure the server has been stopped (not still running outside the server):
```
curl https://10.115.176.36:4443 --insecure
```

View logs:
```
sudo journalctl -u simple-https.service
```