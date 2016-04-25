# meteor-script-backup
Easy backup script for Meteor with MupX (aka Mup with Docker)

## Installation

1. Create a backup directory where you want to store your backup. EG. `/var/lib/mongodb/backup`
2. Do a symbolic link between the docker volume and your directory. EG. `ln -s /var/lib/mongodb/backup /backup`
3. Change the variable of the script to your liking.
  - `containerName` is the name of the MongoDB container.
  - `containerBackupLoc` is the location of the mongoDB backup in your container.
  - `dbName` is the name of the MongoDB base.
  - `cfsLoc` is the location of your CFS directory if needed.
  - `curdate` is the date command format you want to use.
  - `backupLoc` is the directory you've created to store your backup.
4. Set a cron to run the script when you want. EG.

  `0 2 * * 0 sh /backup/backup.sh >> /backup/logs 2>&1` for every monday at 2:00 AM.

5. You should REALLY add a SCP or any other way to extract your backup from the server

## Manual Backup

If your backup are store in `/backup`, as **root**, run the following command:

`sh /backup/backup.sh >> /backup/logs 2>&1`

## Manual Restore
If your backup are store in `/backup`, as root, run the following commands where 11-11-11 is the backup date:

- `tar -xf mongo-11-11-11.tar.gz -C mongo-11-11-11`
- `mongorestore --dir mongo-11-11-11`
- `tar -xf cfs-11-11-11.tar.gz -C cfs-11-11-11`
- `cp -r cfs-11-11-11 /opt/mywebsite/cfs`
- `rm -r mongo-11-11-11 cfs-11-11-11`

To restore in dev environment:
- `mongorestore --dir mongo-11-11-11/SITENAME -h IPADDRESS --port PORT -d DATABASENAME`
