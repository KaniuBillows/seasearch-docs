## Overview

There are generally two parts of data to backup
- SeaSearch index metadata
- SeaSearch index document data

## Backuping up and restore seaseach data
For different backend storage type, there are some differences in the data that needs to be backed up.

There are two backup processes, depending on the configured storage backend:
- backup full data (For local disk storage).
- only backup backup the metadata (For object storage e.g. s3).

### Backing up full data for disk storeage
The data files are all stored in the `SS_DATA_PATH` environment variable directory, so just back up the whole directory. 
You can directly copy the whole directory to the backup destination, or you can use rsync to do incremental backup.

We assume your `SS_DATA_PATH` is `/opt/seasearch/` and you want to backup to `/backup` directory. 

To directly copy the whole data directory:
```
cp -R /opt/seasearch /backup/data/seasearch-`date +"%Y-%m-%d-%H-%M-%S"`
```
This produces a separate copy of the data directory each time. You can delete older backup copies after a new one is completed.

If you have a lot of data, copying the whole data directory would take long. You can use rsync to do incremental backup.
```
rsync -az /opt/seasearch /backup/data/
```

This command backup the data directory to `/backup/data/seasearch`.

### Backuping up metadata for object storage
When SeaSearch is deployed with object stroage backend, you just need backup the metadata file.

We assume your `SS_DATA_PATH` is `/opt/seasearch/` and you want to backup to `/backup` directory. 

To directly copy the metadata file:
```
cp /opt/seasearch/_metadata.bolt /backup/data/seasearch-`date +"%Y-%m-%d-%H-%M-%S"`
```
This produces a separate copy of the data directory each time. You can delete older backup copies after a new one is completed.

### Restore from backup

Now supposed your primary SeaSearch server is broken, you're switching to a new machine. 
Let's assume the seasearch deployment location new machine is also `/opt/seasearch` and the backup files is in `/backup/data/seasearch-2025-01-18-14-35-45`, using the backup data to restore your SeaSearch instance:

#### Restore for disk storage

Just need to copy backup data files to the new machine:
```
cp -R /backup/data/seasearch-2025-01-18-14-35-45/ /opt/seasearch
```

#### Restore for object storage

Just need to copy the backup file to `/opt/seasearch/` and rename it to `_metadata.bolt`:

```
cp /backup/data/seasearch-2025-01-18-14-35-45 /opt/seaserch/_metadata.bolt
```
