🔄 Automated Backup & Rotation Script with Google Drive Integration
📌 Project Overview

This project implements a fully automated backup system that prevents data loss by creating compressed backups, applying a rotation policy, and securely uploading them to Google Drive.

It is designed for real-world scenarios where accidental deletion or system failure can impact production systems.

🎯 Objective
Automate backup of a project directory
Store backups in a structured and timestamped format
Upload backups to Google Drive using CLI tools
Implement rotational backup strategy (daily, weekly, monthly)
Maintain logs and send webhook notifications
🛠️ Tech Stack
Bash / Python Script
rclone (Google Drive integration)
cURL (Webhook notification)
Cron (Scheduling automation)
📂 Project Structure
.
├── backup_script.sh        # Main backup script
├── config.env              # Configuration file (optional)
├── backup.log              # Log file
└── README.md
⚙️ Features

✅ Automated ZIP backups
✅ Timestamp-based naming
✅ Structured local storage
✅ Google Drive upload
✅ Backup rotation policy
✅ Logging system
✅ Webhook notification (optional)

🧾 Backup Format
📦 File Naming:
ProjectName_YYYYMMDD_HHMMSS.zip
📁 Directory Structure:
~/backups/ProjectName/YYYY/MM/DD/
☁️ Google Drive Integration
1️⃣ Install rclone
curl https://rclone.org/install.sh | sudo bash
2️⃣ Configure Google Drive
rclone config
Choose: n (new remote)
Select: drive
Follow authentication steps
3️⃣ Upload Backup
rclone copy /path/to/backup.zip remote:BackupFolderName
🔁 Rotation Policy
📌 Default Retention:
Daily backups → last 7
Weekly backups (Sunday) → last 4
Monthly backups → last 3
⚙️ Configurable via variables:
DAILY_RETENTION=7
WEEKLY_RETENTION=4
MONTHLY_RETENTION=3
🧠 Logic:
Keep recent backups
Delete older backups automatically
Weekly = backups from Sundays only
Monthly = first backup of each month
▶️ How to Run Script
Basic Run:
bash backup_script.sh
With Arguments:
bash backup_script.sh --source /path/to/project --no-notify
Example:
bash backup_script.sh --source /home/ubuntu/app --project MyApp
⚙️ Configuration (Optional)

Create config.env:

PROJECT_NAME=MyApp
SOURCE_DIR=/home/ubuntu/app
BACKUP_DIR=~/backups
GDRIVE_REMOTE=remote:BackupFolder
WEBHOOK_URL=https://webhook.site/your-url

DAILY_RETENTION=7
WEEKLY_RETENTION=4
MONTHLY_RETENTION=3
📅 Cron Job Setup (Automation)

Edit crontab:

crontab -e
Run daily at 2 AM:
0 2 * * * /bin/bash /path/to/backup_script.sh
📜 Logging

Log file: backup.log

Includes:
Backup timestamp
File name
Upload status
Deleted backups summary
Sample Log:
[2026-03-30 02:00:01] Backup created: MyApp_20260330_020001.zip
[2026-03-30 02:00:10] Upload successful
[2026-03-30 02:00:12] Deleted old backups: 3 files
📡 Webhook Notification
cURL Request:
curl -X POST -H "Content-Type: application/json" \
-d '{"project":"MyApp","date":"2026-03-30","status":"BackupSuccessful"}' \
https://webhook.site/your-unique-url
Disable Notification:
--no-notify
📊 Example Workflow
Script runs via cron
Creates ZIP backup
Stores locally in structured folder
Uploads to Google Drive
Deletes old backups (based on policy)
Logs everything
Sends webhook notification
🔐 Security Considerations
Restrict access to backup directories:
chmod 700 ~/backups
Protect config file:
chmod 600 config.env
Avoid exposing:
Google Drive credentials
Webhook URLs
Use IAM-like principle:
Grant minimal access to storage
🚀 Key Learnings
Backup automation in real-world systems
Implementing retention policies
Secure cloud storage integration
Logging and monitoring best practices
🔮 Future Enhancements
Encrypt backups before upload
Add email/SNS notifications
Use incremental backups
Integrate with Docker volumes
Dashboard for backup monitoring
🤝 Conclusion

This project provides a reliable, automated, and scalable backup solution with rotation and cloud integration. It ensures data safety, minimizes downtime risks, and follows best practices for production environments.
