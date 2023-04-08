## promethius installation ##

 wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
 sudo useradd --no-create-home -c "Monitoring user" --shell /bin/false node_exporter
 tar -vxzf node_exporter-1.5.0.linux-amd64.tar.gz
 sudo mv node_exporter-1.5.0.linux-amd64/node_exporter /usr/local/bin/
 sudo chown -R node_exporter:node_exporter  /usr/local/bin/node_exporter
 sudo vim /etc/systemd/system/node_exporter.service
 sudo systemctl daemon-reload
 sudo systemctl start node_exporter
 sudo systemctl status node_exporter
 sudo netstat -tulpn | grep 9100
 
  
  ## promethius mysql-node-exporter installation ##
  
 
 sudo useradd --no-create-home -c "Monitoring user" --shell /bin/false mysqld_exporter
 wget https://github.com/prometheus/mysqld_exporter/releases/download/v0.14.0/mysqld_exporter-0.14.0.linux-amd64.tar.gz
 tar -vxzf mysqld_exporter-0.14.0.linux-amd64.tar.gz
 sudo mv mysqld_exporter-0.14.0.linux-amd64/mysqld_exporter /usr/local/bin/
 sudo chown -R mysqld_exporter:mysqld_exporter  /usr/local/bin/mysqld_exporter
 sudo vim /etc/.mysqld_exporter.cnf
 sudo chown root:mysqld_exporter /etc/.mysqld_exporter.cnf
 sudo vim /etc/systemd/system/mysql_exporter.service
 sudo systemctl daemon-reload
 sudo systemctl enable mysql_exporter
 sudo systemctl start mysql_exporter
 sudo netstat -tulpn | grep 9104
