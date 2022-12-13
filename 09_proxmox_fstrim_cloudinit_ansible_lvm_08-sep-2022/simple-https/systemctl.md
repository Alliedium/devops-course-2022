## Simple HTTPS server creation and management with systemctl ##

### Prerequisites ###

1. We need 3 files in simple-https directory (you can find them in simple-https directory and pass via scp onto your VM):
```
[tatyana@rocky ~]$ cd simple-https/
[tatyana@prodlike-config simple-https]$ ls
example-com.conf  make-cert.sh  simple-server.py
[tatyana@rocky simple-https]$ nano simple-server.py
```

2. We need python to be installed:
```
[tatyana@rocky ~]$ sudo dnf install python3
```

### Creation ###

1. We should run make-cert.sh first in order to generate certificate.

2. We can check if our python script can be executed without errors:
```
[tatyana@rocky simple-https]$ python3 simple-server.py
```
 
3. In order to start it from the service we need following:
```
[tatyana@rocky ~]$ cd /etc/systemd/system/
[tatyana@rocky system]$ sudo nano simple-https.service
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

4. Once you added a new service or made any changes in existing one you should run:
```
[tatyana@rocky system]$ sudo systemctl daemon-reload
```

### Management ###

Then we can start the service:
```
[tatyana@rocky system]$ sudo systemctl start simple-https.service
```

And check our server response:
```
[tatyana@rocky system]$ curl https://10.115.176.36:4443 --insecure
```

Let's stop the service:
```
[tatyana@rocky system]$ sudo systemctl stop simple-https.service
```

Let's make sure the server has been stopped as well 
(make sure it is not still running outside the server):
```
[tatyana@rocky system]$ curl https://10.115.176.36:4443 --insecure
```

View logs:
```
[tatyana@rocky system]$ sudo journalctl -u simple-https.service
```