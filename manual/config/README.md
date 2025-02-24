# SeaSearch Configuration
!!! tip
    After adding new vairables or modifying the existing variables, you have to restart the service to enable changing:

    ```sh
    docker compose down
    docker compose up -d
    ```
  
## Object Storage
`SS_STORAGE_TYPE`: Type of storage medium, default is `disk`. Other possible options are `s3` and `oss` (`oss` option will not show in this document, please refer to [ZincSearch official document](https://zincsearch-docs.zinc.dev/) for the details)

### Local Storage

| Variable | Description | Default |
| --- | --- | --- |
| `SS_MAX_OBJ_CACHE_SIZE` | When using object storage, the maximum local cache size. | 10GB |
| `SS_DATA_PATH` | Local storage path. This is a required option and will be used for local cache storage when using object storage (replaces the original `ZINC_DATA_PATH`). | `./data` |

### S3
These configurations are only effective when `SS_STORAGE_TYPE=s3`.

| Variable | Description | Default |
| --- | --- | --- |
| `SS_S3_ACCESS_ID` | The `SS_S3_ACCESS_ID` is required to authenticate you to S3. You can find the `SS_S3_ACCESS_ID` in the "security credentials" section on your AWS account page or from your storage provider. | <required\> |
| `SS_S3_USE_V4_SIGNATURE` | There are two versions of authentication protocols that can be used with S3 storage: Version 2 (older, may still be supported by some regions) and Version 4 (current, used by most regions). If you don't set this option, SeaSearch will use the v2 protocol. It's suggested to use the v4 protocol. | `false` |
| `SS_S3_ACCESS_SECRET` | This variable is required to authenticate you to S3. You can find the key in the "security credentials" section on your AWS account page or from your storage provider. | <required\> |
| `SS_S3_ENDPOINT` | The endpoint by which you access the storage service. Usually it starts with the region name. It's required to provide the host address if you use storage provider other than AWS, otherwise SeaSearch will use AWS's address (i.e., `s3.us-east-1.amazonaws.com`). | `s3.us-east-1.amazonaws.com` |
| `SS_S3_BUCKET` | Bucket name for SeaSearch storage. Make sure it follows [S3 naming rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/BucketRestrictions.html#bucketnamingrules). | <required\> |
| `SS_S3_USE_HTTPS` | Use https to connect to S3. It's recommended to use https. | true |
| `SS_S3_PATH_STYLE_REQUEST` | This option asks SeaSearch to use URLs like `https://192.168.1.123:8080/bucketname/object` to access objects. In Amazon S3, the default URL format is in virtual host style, such as `https://bucketname.s3.amazonaws.com/object`. But this style relies on advanced DNS server setup. So most self-hosted storage systems only implement the path style format. So we recommend to set this option to `true` for self-hosted storage. | `true` |
| `SS_S3_AWS_REGION` | If you use the v4 protocol and AWS S3, set this option to the region you chose when you create the buckets. If it's not set and you're using the v4 protocol, SeaSearch will use `us-east-1` as the default. This option will be ignored if you use the v2 protocol. | `us-east-1` |
| `SS_S3_SSE_C_KEY` | A string of 32 characters can be generated by `openssl rand -base64 24`. It can be any 32-character long random string. It's required to use V4 authentication protocol and https if you enable SSE-C. | <only required if SSE-C is enabled\> |

## Logging
| Variable | Description | Default |
| --- | --- | --- |
| `SS_LOG_TO_STDOUT` | Whether to output logs to standard output. | `false` |
| `SS_LOG_DIR` | Log directory. | `/opt/seasearch/data/log` (a log subdirectory in the `SS_DATA_PATH` directory) |
| `SS_LOG_LEVEL` | Log level. | `debug` |

## Example SeaSearch Configuration
### Initial Seasearch with Local Disk as Storage Backend
``` sh
  INIT_SS_ADMIN_USER=admin
  INIT_SS_ADMIN_PASSWORD=password
  SS_DATA_PATH=./data
```

### Initial Seasearch with S3 as Storage Backend
=== "AWS"
    ``` sh
    INIT_SS_ADMIN_USER=admin
    INIT_SS_ADMIN_PASSWORD=password
    SS_DATA_PATH=./data
    SS_STORAGE_TYPE=s3
    SS_S3_ACCESS_ID=<your-s3-key-id>
    SS_S3_ACCESS_SECRET=<your-s3-secret-key>
    SS_S3_BUCKET=<your-seasearch-bucket>
    SS_S3_REGION=us-east-1
    SS_S3_USE_HTTPS=true
    SS_S3_USE_V4_SIGNATURE=true
    ```
=== "Exoscale"
    ``` sh
    INIT_SS_ADMIN_USER=admin
    INIT_SS_ADMIN_PASSWORD=password
    SS_DATA_PATH=./data
    SS_STORAGE_TYPE=s3
    SS_S3_ACCESS_ID=<your-s3-key-id>
    SS_S3_ACCESS_SECRET=<your-s3-secret-key>
    SS_S3_BUCKET=<your-seasearch-bucket>
    SS_S3_ENDPOINT=sos-de-fra-1.exo.io
    SS_S3_PATH_STYLE_REQUEST=true
    ```
=== "Hetzner"
    ``` sh
    INIT_SS_ADMIN_USER=admin
    INIT_SS_ADMIN_PASSWORD=password
    SS_DATA_PATH=./data
    SS_STORAGE_TYPE=s3
    SS_S3_ACCESS_ID=<your-s3-key-id>
    SS_S3_ACCESS_SECRET=<your-s3-secret-key>
    SS_S3_BUCKET=<your-seasearch-bucket>
    SS_S3_ENDPOINT=fsn1.your-objectstorage.com
    SS_S3_PATH_STYLE_REQUEST=true
    SS_S3_USE_HTTPS=true
    ```
=== "Other Public Hosted S3 Storag"
    ```sh
    INIT_SS_ADMIN_USER=admin
    INIT_SS_ADMIN_PASSWORD=password
    SS_DATA_PATH=./data
    SS_STORAGE_TYPE=s3
    SS_S3_ACCESS_ID=<your-s3-key-id>
    SS_S3_ACCESS_SECRET=<your-s3-secret-key>
    SS_S3_BUCKET=<your-seasearch-bucket>
    SS_S3_ENDPOINT=<access endpoint for storage provider>
    SS_S3_REGION=<region name for storage provider>
    SS_S3_USE_HTTPS=true
    ```
=== "Self-hosted S3 Storage"
    ```sh
    INIT_SS_ADMIN_USER=admin
    INIT_SS_ADMIN_PASSWORD=password
    SS_DATA_PATH=./data
    SS_STORAGE_TYPE=s3
    SS_S3_ACCESS_ID=<your-s3-key-id>
    SS_S3_ACCESS_SECRET=<your-s3-secret-key>
    SS_S3_BUCKET=<your-seasearch-bucket>
    SS_S3_ENDPOINT=<your s3 api endpoint host>:<your s3 api endpoint port>
    SS_S3_USE_HTTPS=true
    SS_S3_PATH_STYLE_REQUEST=true
    SS_S3_USE_HTTPS=true
    ```
