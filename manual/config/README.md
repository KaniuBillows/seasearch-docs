# SeaSearch Configuration

## Single-Node Configurations

### Basic Configurations

```shell
# log mode of gin framework，default release
ZINC_WAL_ENABLE=true

# type of storage's engine, i.e., s3
ZINC_STORAGE_TYPE=

# the number of shards, since seaseach has one index per database, in order to improve loading efficiency, the default value is changed to 1
ZINC_SHARD_NUM=1
```

### S3 Storage Configurations

To enable s3 storage configurations, the term `ZINC_STORAGE_TYPE` has to be set as `ZINC_STORAGE_TYPE=s3`.

```shell
# the maximum local cache file size
ZINC_MAX_OBJ_CACHE_SIZE=

# S3 relative informations
ZINC_S3_ACCESS_ID=<your s3 access id>
ZINC_S3_USE_V4_SIGNATURE=<your s3 signature>
ZINC_S3_ACCESS_SECRET=<your s3 access secret>
ZINC_S3_ENDPOINT=<your s3 endpoint>
ZINC_S3_USE_HTTPS=<your s3 tls enabled>
ZINC_S3_PATH_STYLE_REQUEST=<your s3 style request path>
ZINC_S3_AWS_REGION=<your s3 AWS region>
```

## Logs Configurations

```shell
ZINC_LOG_OUTPUT=true #whether to output logs to files, default yes
ZINC_LOG_DIR=/opt/seasearch/data/log #log directory
ZINC_LOG_LEVEL=debug #log level，default debug
```
