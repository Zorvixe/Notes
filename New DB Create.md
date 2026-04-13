✅ 1. Login to PostgreSQL
sudo -u postgres psql
✅ 2. Create a New Database User (Recommended)

👉 It’s best practice to use separate users per project

CREATE USER newappuser WITH ENCRYPTED PASSWORD 'new_strong_password';
✅ 3. Create a New Database
CREATE DATABASE newappdb OWNER newappuser;
✅ 4. Grant Privileges (optional but good)
GRANT ALL PRIVILEGES ON DATABASE newappdb TO newappuser;
✅ 5. Exit
\q
✅ 6. Test Connection
Local:
psql -d newappdb -U newappuser
Remote:
psql -h your_vps_ip -p 5432 -d newappdb -U newappuser
🔗 Connection URI for New Project
postgresql://newappuser:new_strong_password@localhost:5432/newappdb
⚠️ Important Best Practices (Very Important)

Since you're running production:

❌ Don’t reuse same DB user for all apps

✔ Use:

app1_user → app1_db
app2_user → app2_db
🔒 Restrict Remote Access (VERY IMPORTANT)

Instead of:

0.0.0.0/0

Use your IP:

host    all    all    YOUR_IP/32    scram-sha-256
💾 Backup Each Database Separately
pg_dump -U newappuser newappdb > newappdb_backup.sql
🚀 If You Want Faster Setup (Shortcut)

If you don’t care about separate users:

CREATE DATABASE newdb;
GRANT ALL PRIVILEGES ON DATABASE newdb TO existing_user;
