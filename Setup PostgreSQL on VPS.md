Setting Up PostgreSQL Database

Install PostgreSQL and useful extensions

sudo apt update
sudo apt install -y postgresql postgresql-contrib

Check PostgreSQL status

sudo systemctl status postgresql
8. Secure PostgreSQL Superuser

Login to PostgreSQL shell

sudo -u postgres psql

Set strong password for postgres user

ALTER USER postgres WITH PASSWORD 'YourStrongPasswordHere';

Exit shell

\q
9. Create Database & User for Project

Login again

sudo -u postgres psql

Create new user

CREATE USER appuser WITH ENCRYPTED PASSWORD 'app_password';

Create database

CREATE DATABASE appdb OWNER appuser;

Grant privileges

GRANT ALL PRIVILEGES ON DATABASE appdb TO appuser;

Exit shell

\q
10. Allow PostgreSQL Through Firewall
sudo ufw status

If firewall is disabled, enable it:

sudo ufw enable

Allow required ports

sudo ufw allow 5432/tcp
sudo ufw allow 'OpenSSH'
11. Enable Remote Connections (Optional)

Check PostgreSQL version

ls /etc/postgresql/

Edit configuration file (replace version if needed)

sudo nano /etc/postgresql/16/main/postgresql.conf

Update this line

listen_addresses = '*'

Edit client authentication file

sudo nano /etc/postgresql/16/main/pg_hba.conf

Add this line at bottom

host    all             all             0.0.0.0/0               scram-sha-256

Restart PostgreSQL

sudo systemctl restart postgresql
12. Connect to PostgreSQL

Local connection

psql -d appdb -U appuser

Remote connection

psql -h your_vps_ip -p 5432 -d appdb -U appuser
🗄️ PostgreSQL Connection URI

Local:

postgresql://appuser:app_password@localhost:5432/appdb

Remote:

postgresql://appuser:app_password@your_vps_ip:5432/appdb
⚡ Pro Tips (Important for Production)

👉 Instead of 0.0.0.0/0, use your IP:

your-ip-address/32

👉 Never expose DB publicly without firewall restriction
👉 Always use strong passwords
👉 Keep backups (very important)

🔥 BONUS (For Your Level 🚀)

Since you're building client systems, you can position it like:

💼 Database Setup + Hosting + Security
₹1500–₹3000/month recurring

If you want next level:

✅ PostgreSQL + Node.js (Prisma / Sequelize setup)
✅ Auto backup scripts
✅ pgAdmin setup (UI like MongoDB Compass)
✅ Full production architecture (Nginx + DB + Backend)

Just say:
👉 “add postgres to my MERN deployment fully”

I’ll upgrade your whole doc into agency-level professional README 🔥
