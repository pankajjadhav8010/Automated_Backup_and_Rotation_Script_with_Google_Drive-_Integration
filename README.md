# Automated Backup and Rotation Script with Google Drive Integration

## Project Overview

This project implements an automated backup system designed to prevent data loss by creating compressed backups, applying a rotation policy, and uploading them securely to Google Drive.

The solution is suitable for production environments where system failures or accidental deletions can lead to data loss. It ensures that backups are consistently created, stored, and managed without manual intervention.

## Objective

* Automate backup of a project directory
* Store backups using a structured and timestamp-based format
* Upload backups to Google Drive using command-line tools
* Implement a rotation policy for efficient storage management
* Maintain logs and support optional webhook notifications

## Technology Stack

* Bash scripting
* rclone (Google Drive integration)
* cURL (Webhook notifications)
* Cron (Scheduling)

## Project Structure

```
.
├── backup_script.sh
├── config.env
├── backup.log
└── README.md
```

## Features

* Automated compressed backups
* Timestamp-based file naming
* Structured directory storage
* Integration with Google Drive
* Backup rotation policy
* Logging system
* Optional webhook notifications

## Backup Format

### File Naming

```
ProjectName_YYYYMMDD_HHMMSS.zip
```

### Directory Structure

```
~/backups/ProjectName/YYYY/MM/DD/
```

## Google Drive Integration

### Install rclone

```
curl https://rclone.org/install.sh | sudo bash
```

### Configure Google Drive

```
rclone config
```

* Create a new remote
* Select "drive" as storage type
* Complete authentication

### Upload Backup

```
rclone copy /path/to/backup.zip remote:BackupFolderName
```

## Rotation Policy

### Default Retention

* Daily backups: retain last 7
* Weekly backups (Sunday): retain last 4
* Monthly backups: retain last 3

### Configurable Variables

```
DAILY_RETENTION=7
WEEKLY_RETENTION=4
MONTHLY_RETENTION=3
```

### Logic

* Retain recent backups based on defined limits
* Automatically delete older backups
* Weekly backups are identified from Sunday runs
* Monthly backups are selected as the first backup of each month

## Script Execution

### Basic Run

```
bash backup_script.sh
```

### With Arguments

```
bash backup_script.sh --source /path/to/project --no-notify
```

### Example

```
bash backup_script.sh --source /home/ubuntu/app --project MyApp
```

## Configuration (Optional)

Create a configuration file:

```
PROJECT_NAME=MyApp
SOURCE_DIR=/home/ubuntu/app
BACKUP_DIR=~/backups
GDRIVE_REMOTE=remote:BackupFolder
WEBHOOK_URL=https://webhook.site/your-url

DAILY_RETENTION=7
WEEKLY_RETENTION=4
MONTHLY_RETENTION=3
```

## Automation with Cron

Edit crontab:

```
crontab -e
```

Add the following line to run daily at 2 AM:

```
0 2 * * * /bin/bash /path/to/backup_script.sh
```

## Logging

Log file: `backup.log`

### Log Contents

* Backup timestamp
* Backup file name
* Upload status
* Deleted backups summary

### Sample Log

```
[2026-03-30 02:00:01] Backup created: MyApp_20260330_020001.zip
[2026-03-30 02:00:10] Upload successful
[2026-03-30 02:00:12] Deleted old backups: 3 files
```

## Webhook Notification

### Example Request

```
curl -X POST -H "Content-Type: application/json" \
-d '{"project":"MyApp","date":"2026-03-30","status":"BackupSuccessful"}' \
https://webhook.site/your-unique-url
```

### Disable Notifications

Use the following flag:

```
--no-notify
```

## Workflow

1. Script runs via cron scheduler
2. Creates compressed backup of the source directory
3. Stores backup in structured local directory
4. Uploads backup to Google Drive
5. Applies rotation policy and removes old backups
6. Logs all actions
7. Sends webhook notification if enabled

## Security Considerations

* Restrict access to backup directories
* Protect configuration files using proper permissions
* Avoid exposing credentials and webhook URLs
* Grant minimal required access to storage systems

## Optional Enhancements

* Encrypt backups before uploading
* Add email or notification integrations
* Implement incremental backups
* Integrate with containerized environments
* Build a monitoring dashboard

## Key Learnings

* Automating backup processes in production systems
* Implementing retention and rotation strategies
* Integrating cloud storage with command-line tools
* Managing logs and notifications effectively

## Conclusion

This project provides a reliable and automated backup solution with structured storage, rotation policies, and cloud integration. It helps ensure data availability, reduces risk of data loss, and aligns with best practices for system reliability and maintenance.
