## UrBackup_client_pre_post_backup_scripts_template

Pre and post backup scripts template for UrBackup client. I tested it with 2.4.10 at debian 10.
UrBackup documentation: https://www.urbackup.org/administration_manual.html#x1-330006.4

I use these scripts for stop services before backup and start them back after snapshot creation.

### Features

* Each command is checked for errors;

* If errors occur during operation, the script breaks, the stopped services will be started services back, then UrBackup backup is not started (Backup had an early error. Deleting partial backup);

* All events are included in the UrBackup log;

* Errors are logged in more detail;

### How to use

* Put these scripts to `/usr/local/etc/urbackup` and make them executable;

* Edit prefilebackup, postfileindex, postfilebackup (there are inctructions inside);

* Create file backup and check log (at the web interface).

### Scripts description

* `prefilebackup` - UrBackup called before a file backup (before snapshot/shadowcopy creation);

* `postfileindex` - UrBackup called it after indexing (after snapshot/shadowcopy creation);

* `functions_prepost_scripts` - functions used by these sripts.

### Log examples
**Log without errors:**

|Level|Time|Message|
|---|---|---|
|Info|29.04.2020 15:08|Starting unscheduled incremental file backup...|
|Info|29.04.2020 15:08|**Starting prefilebackup script**|
|Info|29.04.2020 15:08|run: systemctl stop fail2ban|
|Info|29.04.2020 15:08|**prefilebackup succeeded**|
|Info|29.04.2020 15:08|Snapshotting device /dev/mapper/vgroot-root via dattobd...|
|Info|29.04.2020 15:08|Using /dev/datto0...|
|Info|29.04.2020 15:08|Mounting /dev/mapper/wsnap-76696aae351ab3313541b94beed0148caf9b7a4e86d22e40...|
|Info|29.04.2020 15:08|Indexing of "rootfs" done. 8376 filesystem lookups 0 db lookups and 0 db updates|
|Info|29.04.2020 15:08|Snapshotting device /dev/sda3 via dattobd...|
|Info|29.04.2020 15:08|Using /dev/datto1...|
|Info|29.04.2020 15:08|Mounting /dev/mapper/wsnap-10fedf2eed26fda4f9a17801428e72855aa0efe35d3884fb...|
|Info|29.04.2020 15:08|Indexing of "boot" done. 6 filesystem lookups 0 db lookups and 0 db updates|
|Info|29.04.2020 15:08|**Starting postfileindex script** (normally starts after indexing and snapshot creation)|
|Info|29.04.2020 15:08|run: systemctl start fail2ban|
|Info|29.04.2020 15:08|**postfileindex succeeded**|
|Info|29.04.2020 15:08|UrBackupC2dattobd: Loading file list...|
|Info|29.04.2020 15:08|UrBackupC2dattobd: Calculating file tree differences...|
|Info|29.04.2020 15:08|UrBackupC2dattobd: Creating snapshot...|
|Info|29.04.2020 15:08|UrBackupC2dattobd: Deleting files in snapshot... (14)|
|Info|29.04.2020 15:08|UrBackupC2dattobd: Deleting files in hash snapshot...(14)|
|Info|29.04.2020 15:08|UrBackupC2dattobd: Calculating tree difference size...|
|Info|29.04.2020 15:08|UrBackupC2dattobd: Linking unchanged and loading new files...|
|Info|29.04.2020 15:08|Waiting for file transfers...|
|Info|29.04.2020 15:08|Waiting for file hashing and copying threads...|
|Info|29.04.2020 15:09|Writing new file list...|
|Info|29.04.2020 15:09|All metadata was present|
|Info|29.04.2020 15:09|Transferred 12.4891 MB - Average speed: 51.1805 MBit/s|
|Info|29.04.2020 15:09|Time taken for backing up client UrBackupC2dattobd: 1m 10s|
|Info|29.04.2020 15:09|Backup succeeded|

**Log with errors:**
|Level|Time|Message|
|---|---|---|
|Info|29.04.2020 17:23|Starting unscheduled incremental file backup...|
|Errors|29.04.2020 17:23|**Starting prefilebackup script**|
|Errors|29.04.2020 17:23|run: systemctl stop fail22ban|
|Errors|29.04.2020 17:23|Failed to stop fail22ban.service: Unit fail22ban.service not loaded.|
|Errors|29.04.2020 17:23|prefilebackup script on client returned error on command: systemctl stop fail22ban. Return code: 5|
|Errors|29.04.2020 17:23|UrBackup will not run postfileindex script and stopped services will not be started.|
|Errors|29.04.2020 17:23|Fix it. Run postfileindex script right now for **start services back**|
|Errors|29.04.2020 17:23|Starting postfileindex script (normally starts after indexing and snapshot creation)|
|Errors|29.04.2020 17:23|run: systemctl start fail2ban|
|Errors|29.04.2020 17:23|postfileindex succeeded|
|Errors|29.04.2020 17:23|**prefilebackup script was interrupted**!|
|Errors|29.04.2020 17:23|Constructing of filelist of "UrBackupC2dattobd" failed: error - prefilebackup script failed with error code 256|
|Errors|29.04.2020 17:23|Backup had an early error. Deleting partial backup.|
