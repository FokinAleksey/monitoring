## Конфигуратор инструкции по установке Zabbix можно найти по ссылке: [Zabbix](https://www.zabbix.com/ru/download?zabbix=6.0&os_distribution=debian&os_version=11&components=server_frontend_agent&db=pgsql&ws=apache)

### Установка Zabbix Server на Debian 11

*Установите PostgreSQL*
sudo apt install postgresql
# Добавьте репозиторий Zabbix
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb dpkg -i zabbix-release_6.0-4+debian11_all.deb
apt update
# Запустите Zabbix Server
sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts nano -y # zabbix-agent
# Создайте пользователя БД
sudo -u postgres createuser --pwprompt zabbix
# Создайте БД
sudo -u postgres createdb -O zabbix zabbix


``` 
sudo apt install postgresql 
wget https://repo.zabbix.com/zabbix/6.0/debian/pool/main/z/zabbix-release/zabbix-release_6.0-4%2Bdebian11_all.deb
sudo dpkg -i zabbix-release_6.0-4+debian11_all.deb
sudo apt update
sudo apt install zabbix-server-pgsql zabbix-frontend-php php7.4-pgsql zabbix-apache-conf zabbix-sql-scripts -y 
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix 
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix 
sudo nano /etc/zabbix/zabbix_server.conf 
DBPassword=123456789
sudo systemctl restart zabbix-server apache2 
sudo systemctl enable zabbix-server apache2 
sudo systemctl status zabbix-server 
sudo systemctl status apache2
```


Установите Zabbix Agent на два хоста.





---
### 
![ ](image/1.png)
![ ](image/2.png)
