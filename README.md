# PostgreSQL Database Backup Automation

## Overview
This script automates PostgreSQL database backups with compression, retention management, and Telegram notifications.

## Features
- Automated backups of all PostgreSQL databases
- Backup compression using gzip
- Configurable retention policy
- Telegram notifications for failure
- Detailed logging

## Requirements

- Python 3 (`/usr/bin/python3`)

## Configuration

###  Script Location
1. Create script directory:
```bash
mkdir /crystal/
cd /crystal/
```
2. Create backup script:
```bash
vim pgdb_backup.py

```
### Please modify the `pgdb_backup.py` file according to your requirements.

3. Make script executable:

```bash
chmod +x pgdb_backup.py
```

> ⚠️ **Keep sensitive data secure. Avoid hardcoding passwords and tokens in production.**
--- 

## 🗓️ Cronjob Setup (Daily at 10:00 PM)

### Edit crontab:
```bash
crontab -e
```

### Add line:
```bash
0 22 * * * /usr/bin/python3 /crystal/pgdb_backup.py >> /var/log/cron_pg_backup.log 2>&1
```

#### Meaning:
- `0 22 * * *`: At 10:00 PM every day
- `>> /var/log/cron_pg_backup.log 2>&1`: Append output and error to cron log

---

## 📂 Backup Output

- Location: `/var/backups/pgdb/db_backup-YYYY-MM-DD_HH-MM-SS/`
- File format: `<dbname>_YYYY-MM-DD_HH-MM-SS.backup.gz`

### Backup Structure
Backups are stored with timestamped folders:

```
/var/backups/pgdb/
└── db_backup-2023-12-31_22-00-00/
    ├── database1_2023-12-31_22-00-00.backup.gz
    └── database2_2023-12-31_22-00-00.backup.gz
```

## Logging
Detailed logs are maintained at:
- /var/log/pgdb_backup.log (`script logs`)
- /var/log/cron_pg_backup.log (`cron job logs`)
---


## Telegram Notification

### Error Handling
The script provides detailed error notifications via Telegram, including:
- Connection failures
- Backup failures for individual databases
- Compression issues
- Retention cleanup problems

Example error notification:

```
Postgres Backup Error:
Failed to get database list:
psql: error: connection to server at "localhost" (::1), port 5432 failed: FATAL:  password authentication failed for user "postgres"
connection to server at "localhost" (::1), port 5432 failed: FATAL:  password authentication failed for user "postgres"
```
