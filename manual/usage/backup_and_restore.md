## Overview

There are generally two parts of data to backup
- SeaSearch index metadata
- SeaSearch index document data

The index metadata is saved in a local BoltDB file by default. The document data for the indexes is saved either to local storage or object storage.

## Backup
For different backend storage types, there are some differences in the data that needs to be backed up.

There are two backup strategies, depending on the configured storage backend:
- If you use local storage, you need to backup both index metadata and document data
- If you use object storage, you only need to backup index metadata. Whether and how the document data needs backup depends on your object storage setup.

We assume your `SS_DATA_PATH` is `/opt/seasearch/` and you want to backup to `/backup` directory. 

### Backup strategy for local storage

The data files are all stored in the directory specified by `SS_DATA_PATH` environment variable, so just back up the whole directory. 
You can directly copy the whole directory to the backup destination, or you can use rsync to do incremental backup.

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

### Backup strategy for object storage

When SeaSearch is deployed with object storage backend, you only need to back up the metadata file.

To directly copy the metadata file:
```
cp /opt/seasearch/_metadata.bolt /backup/data/seasearch-metadata.bolt.`date +"%Y-%m-%d-%H-%M-%S"`
```
This produces a separate copy of the data directory each time. You can delete older backup copies after a new one is completed.

## Restore

Now suppose your primary SeaSearch server has failed, and you are switching to a new machine.

Assume that the deployment location of SeaSearch on the new machine is also `/opt/seasearch`, and the backup files are located at `/backup/data/seasearch-2025-01-18-14-35-45`.

### Restore strategy for local storage

You only need to copy the backup data files to the new machine:
```
cp -R /backup/data/seasearch-2025-01-18-14-35-45/ /opt/seasearch
```

### Restore strategy for object storage

Simply copy the backup file to `/opt/seasearch/` and rename it to `_metadata.bolt`:

```
cp /backup/data/seasearch-metadata.bolt.2025-01-18-14-35-45 /opt/seaserch/_metadata.bolt
```
