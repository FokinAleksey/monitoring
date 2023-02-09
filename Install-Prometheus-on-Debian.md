# Install Prometheus on Debian 

## install gosu
_just to not wrestle with sudo and exec_
```bash
$ sudo apt-get update
$ sudo apt-get install gosu
```

## Create prometheus user
```bash
$ sudo useradd -M prometheus
$ sudo usermod -L prometheus
```

## Make scaffolding
```bash
$ sudo sudo mkdir /etc/prometheus
$ sudo sudo mkdir /var/lib/prometheus
$ sudo chown prometheus:prometheus /etc/prometheus
$ sudo chown prometheus:prometheus /var/lib/prometheus
```

## Download prometheus
```bash
$ cd ~
$ wget https://github.com/prometheus/prometheus/releases/download/v2.0.0/prometheus-2.0.0.linux-amd64.tar.gz
$ sha256sum prometheus-2.0.0.linux-amd64.tar.gz 
e12917b25b32980daee0e9cf879d9ec197e2893924bd1574604eb0f550034d46  prometheus-2.0.0.linux-amd64.tar.gz
$ tar xvf prometheus-2.0.0.linux-amd64.tar.gz
```

## Put bits of prometheus in places
```bash
$ sudo cp prometheus-2.0.0.linux-amd64/prometheus /usr/local/bin/
$ sudo cp prometheus-2.0.0.linux-amd64/promtool /usr/local/bin/
$ sudo chown prometheus:prometheus /usr/local/bin/prometheus
$ sudo chown prometheus:prometheus /usr/local/bin/promtool
```

```bash
$ sudo cp -r prometheus-2.0.0.linux-amd64/prometheus.yml /etc/prometheus/
$ sudo cp -r prometheus-2.0.0.linux-amd64/consoles /etc/prometheus
$ sudo cp -r prometheus-2.0.0.linux-amd64/console_libraries /etc/prometheus
$ sudo chown -R prometheus:prometheus /etc/prometheus/prometheus.yml
$ sudo chown -R prometheus:prometheus /etc/prometheus/consoles
$ sudo chown -R prometheus:prometheus /etc/prometheus/console_libraries
```

## Configure prometheus
```bash
$ sudo vi /etc/prometheus/prometheus.yml
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

## Test if prometheus runs
```bash
$ sudo -u prometheus /usr/local/bin/prometheus --config.file=/etc/prometheus/prometheus.yml --storage.tsdb.path=/var/lib/prometheus/data --web.console.templates=/etc/prometheus/consoles --web.console.libraries=/etc/prometheus/consoles_libraries 
...
level=info ts=2018-01-17T17:34:29.810960616Z caller=main.go:371 msg="Server is ready to receive requests."
```
_Ctrl+C_