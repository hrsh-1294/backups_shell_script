# Backup Script with 5-Day Rotation

This repository provides a simple **Bash script** for backing up files or directories with automatic rotation.  
The script creates compressed (`.zip`) backups of a source directory and maintains only the latest **5 backups**, automatically deleting older ones.

---

## Features
- Creates timestamped `.zip` backups of any directory.  
- Stores backups in a specified destination folder.  
- Maintains only the **latest 5 backups** (older ones are removed automatically).  
- Provides usage instructions if run incorrectly.  

---

## Prerequisites
- Linux/Unix system with **bash**  
- `zip` command installed (check with `zip --version`)  

---

## Usage
```bash
./backup.sh <path-to-source> <path-to-backup-folder>
```

### Example
```bash
./backup.sh /home/user/projects /home/user/backups
```

This will:
- Create a backup file like:  
  `/home/user/backups/backup_2025-08-17-18-45-23.zip`  
- Ensure only the 5 most recent backups are kept.  

---

## Rotation Policy
- The script sorts backups by creation time (newest first).  
- If there are more than 5 backups, older ones are deleted.  
- Example:
  ```
  backup_2025-08-12-10-15-20.zip
  backup_2025-08-13-11-20-40.zip
  backup_2025-08-14-09-50-00.zip
  backup_2025-08-15-18-30-10.zip
  backup_2025-08-16-22-10-05.zip
  backup_2025-08-17-18-45-23.zip   <-- new backup created
  ```
  After rotation, the oldest (`2025-08-12...`) is deleted.  

---

## Cron Job Setup (Automated Backups at 5:00 PM Daily)

To automate backups, you can schedule the script with **cron**.  

1. Open the cron editor:
   ```bash
   crontab -e
   ```

2. Add the following line to run the backup every day at **5:00 PM**:
   ```bash
   0 17 * * * /path/to/backup.sh /path/to/source /path/to/backup >> /path/to/backup/backup.log 2>&1
   ```

   - `0 17 * * *` → means at **17:00 (5 PM)** every day.  
   - `>> /path/to/backup/backup.log 2>&1` → saves logs and errors to `backup.log` for monitoring.  

3. Save and exit the editor.  
   The script will now run automatically at **5 PM daily**.  

---

## Notes
- Make the script executable before use:
  ```bash
  chmod +x backup.sh
  ```
- Ensure you have write permissions to the backup directory.  
- Check `backup.log` to verify cron execution.  
- Tested on Ubuntu, should work on most Unix-like systems.  
