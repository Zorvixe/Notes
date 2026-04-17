# 🐘 PostgreSQL Setup on VPS (Production Ready)

## 7. Setting Up PostgreSQL Database

Install PostgreSQL and useful extensions

```bash
sudo apt update
sudo apt install -y postgresql postgresql-contrib
```

Check PostgreSQL status

```bash
sudo systemctl status postgresql
```

---

## 8. Secure PostgreSQL Superuser

Login to PostgreSQL shell

```bash
sudo -u postgres psql
```

Set strong password for postgres user

```sql
ALTER USER postgres WITH PASSWORD 'YourStrongPasswordHere';
```

Exit shell

```bash
\q
```

---

## 9. Create Database & User for Project

Login again

```bash
sudo -u postgres psql
```

Create new user

```sql
CREATE USER appuser WITH ENCRYPTED PASSWORD 'app_password';
```

Create database

```sql
CREATE DATABASE appdb OWNER appuser;
```

Grant privileges

```sql
GRANT ALL PRIVILEGES ON DATABASE appdb TO appuser;
```

Exit shell

```bash
\q
```

---

## 10. Allow PostgreSQL Through Firewall

```bash
sudo ufw status
```

If firewall is disabled, enable it:

```bash
sudo ufw enable
```

Allow required ports

```bash
sudo ufw allow 5432/tcp
sudo ufw allow 'OpenSSH'
```

---

## 11. Enable Remote Connections (Optional)

Check PostgreSQL version

```bash
ls /etc/postgresql/
```

Edit configuration file (replace version if needed)

```bash
sudo nano /etc/postgresql/16/main/postgresql.conf
```

Update this line

```conf
listen_addresses = '*'
```

---

Edit client authentication file

```bash
sudo nano /etc/postgresql/16/main/pg_hba.conf
```

Add this line at bottom

```conf
host    all             all             0.0.0.0/0               scram-sha-256
```

Restart PostgreSQL

```bash
sudo systemctl restart postgresql
```

---

## 12. Connect to PostgreSQL

Local connection

```bash
psql -d appdb -U appuser
```

Remote connection

```bash
psql -h your_vps_ip -p 5432 -d appdb -U appuser
```

---

```bash
nano /etc/postgresql/*/main/pg_hba.conf
```

👉 Change this line:
local   all             all                                     peer

👉 To:

local   all             all                                     md5


```bash
nano /etc/postgresql/*/main/postgresql.conf
```

Find:

#listen_addresses = 'localhost'

👉 Change to:

listen_addresses = '*'


```bash
nano /etc/postgresql/*/main/pg_hba.conf
```

Add this line at the bottom:

host    all             all             0.0.0.0/0               md5

👉 This allows connections from anywhere (we can secure later)

```bash
systemctl restart postgresql
```

Step 4: Check if PostgreSQL is listening

Run:

```bash
ss -nltp | grep 5432
```

👉 You should see:

0.0.0.0:5432

## 🗄️ PostgreSQL Connection URI

Local:

```text
postgresql://appuser:app_password@localhost:5432/appdb
```

Remote:

```text
postgresql://appuser:app_password@your_vps_ip:5432/appdb
```

---

## ⚡ Pro Tips

- Use strong passwords
- Restrict access using your IP instead of 0.0.0.0/0
- Always maintain backups
