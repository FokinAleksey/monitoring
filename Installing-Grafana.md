## ***Installing Grafana***
### *Download and install the DEB package:*
```bash
wget https://dl.grafana.com/oss/release/grafana_9.2.4_amd64.deb
$ sudo dpkg -i grafana_9.2.4_amd64.deb
```
### *Enable autostart and start the Grafana server:*
```bash
$ sudo systemctl enable grafana-server
$ sudo systemctl start grafana-server
$ sudo systemctl status grafana-server
```
### *Check the status by connecting to the address:*
https://localhost:3000

### *Standard login and password admin/admin*
---