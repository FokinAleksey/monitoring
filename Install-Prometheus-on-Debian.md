# **Install Prometheus and Node Exporter on Debian** 
---
## ***Install Prometheus:***
### *Create prometheus user:*
```bash
$ sudo useradd -M prometheus
$ sudo usermod -L prometheus
```
### *Create directories and transfer file permissions to the Prometheus user:*
```bash
$ sudo sudo mkdir /etc/prometheus
$ sudo sudo mkdir /var/lib/prometheus
$ sudo chown prometheus:prometheus /etc/prometheus
$ sudo chown prometheus:prometheus /var/lib/prometheus
```
### *Extract the archive and copy the files to the necessary directories:*
### *Download prometheus [GitHub](https://github.com/prometheus/prometheus/releases/)*
```bash
$ cd ~
$ wget https://github.com/prometheus/prometheus/releases/download/v2.0.0/prometheus-2.42.0.linux-amd64.tar.gz
$ sha256sum prometheus-2.0.0.linux-amd64.tar.gz 
e12917b25b32980daee0e9cf879d9ec197e2893924bd1574604eb0f550034d46  prometheus-2.0.0.linux-amd64.tar.gz
$ tar xvf prometheus-2.0.0.linux-amd64.tar.gz
$ sudo cp prometheus-2.0.0.linux-amd64/prometheus /usr/local/bin/
$ sudo cp prometheus-2.0.0.linux-amd64/promtool /usr/local/bin/
$ sudo cp -r prometheus-2.0.0.linux-amd64/prometheus.yml /etc/prometheus/
$ sudo cp -r prometheus-2.0.0.linux-amd64/consoles /etc/prometheus
$ sudo cp -r prometheus-2.0.0.linux-amd64/console_libraries /etc/prometheus
```
### *Transfer file permissions to the Prometheus user:*
```bash
$ sudo chown -R prometheus:prometheus /etc/prometheus/prometheus.yml
$ sudo chown -R prometheus:prometheus /etc/prometheus/consoles
$ sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
$ sudo chown prometheus:prometheus /usr/local/bin/prometheus
$ sudo chown prometheus:prometheus /usr/local/bin/promtool
```
### *Configure prometheus:*
```bash
$ sudo nano /etc/prometheus/prometheus.yml
```
```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'prometheus'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9090']
```

### *Test if prometheus runs*
```bash
$ sudo -u prometheus /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus/data --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/consoles_libraries 
...
level=info ts=2018-01-17T17:34:29.810960616Z caller=main.go:371 msg="Server is ready to receive requests."
```
*_Ctrl+C_*
### *Creating a service to work with Prometheus*
```bash
$ sudo nano /etc/systemd/system/prometheus.service
```
```bash
[Unit]
Description=Prometheus Service
After=network.target
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
--config.file /etc/prometheus/prometheus.yml \
--storage.tsdb.path /var/lib/prometheus/ \
--web.console.templates=/etc/prometheus/consoles \
--web.console.libraries=/etc/prometheus/console_libraries
ExecReload=/bin/kill -HUP $MAINPID Restart=on-failure
[Install]
WantedBy=multi-user.target
```
### *Write autorun. Start the service: Check service status:*
```bash
$ sudo systemctl enable prometheus
$ sudo systemctl start prometheus
$ sudo systemctl status prometheus
```
---
## ***Install Node Exporter:***
### *Download Node Exporter [GitHub](https://github.com/prometheus/node_exporter/releases)*

### *Download the archive from Node Exporter and extract it:*
```bash
$ wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
$ tar xvfz node_exporter-1.5.0.linux-amd64.tar.gz
```
### *Go to the folder that appears:*
```bash
$ cd cd node_exporter-1.5.0.linux-amd64/
```
### *Start node_exporter*
```bash
$ ./node_exporter  
```
 ### *Checking availability:* 
 http://localhost:9100/metrics

### *Copy Node Exporter to Prometheus folder:*
```bash
$ sudo mkdir /etc/prometheus/node-exporter
$ sudo cp ./* /etc/prometheus/node-exporter
```
### *Transfer folder permissions to Prometheus user:*
```bash
$ sudo chown -R prometheus:prometheus /etc/prometheus/node-exporter/
```
### *Creating a service to work with Node Exporter:*
```bash
$ sudo nano /etc/systemd/system/node-exporter.service
``` 
```
[Unit]
Description=Node Exporter
After=network.target
[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/etc/prometheus/node-exporter/node_exporter
[Install]
WantedBy=multi-user.target 
```
### *Write autorun. Start the service: Check service status:*
```bash
$ sudo systemctl enable node-exporter
$ sudo systemctl start node-exporter
$ sudo systemctl status node-exporter
```
---