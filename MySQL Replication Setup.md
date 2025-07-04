# MySQL Replication Setup (Manual - Master to Slave)

This document outlines the exact steps to configure MySQL master-slave replication using a single-database logical dump and binlog position method.

---

## ✅ On Master Server

### 1. Lock Master
```sql
mysql -u root -p
```
Then inside the MySQL shell:
```sql
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
```

📌 **Keep this terminal open** to retain the read lock.

Record the output:
```
File: mysql-bin.000001
Position: 48808024
```

---

### 2. Take a Compressed Backup
In a **new terminal**:
```bash
mysqldump -u root -p --single-transaction your_db_name | gzip > your_db_name_backup.sql.gz
```

---

### 3. Unlock Master
Back in the MySQL session:
```sql
UNLOCK TABLES;
EXIT;
```

---

## ✅ On Slave Server

### 4. Copy Backup File to Slave
```bash
scp your_db_name_backup.sql.gz user@slave_ip:/tmp/
```

---

### 5. Restore the Backup
```bash
gunzip < /tmp/your_db_name_backup.sql.gz | mysql -u root -p
```

---

### 6. Configure Replication
Login to MySQL:
```bash
mysql -u root -p
```

Then run:
```sql
STOP SLAVE;

CHANGE MASTER TO
  MASTER_HOST='192.168.1.10',
  MASTER_USER='replica_user',
  MASTER_PASSWORD='replica_password',
  MASTER_LOG_FILE='mysql-bin.000001',
  MASTER_LOG_POS=48808024;
```

Replace:
- `192.168.1.10` with your master server IP
- `replica_user` with a user that has `REPLICATION SLAVE` privileges

---

### 7. Start Replication
```sql
START SLAVE;
```

---

### 8. Verify Replication
```sql
SHOW SLAVE STATUS\G
```

✅ Look for:
```
Slave_IO_Running: Yes
Slave_SQL_Running: Yes
Seconds_Behind_Master: 0
```
4m

