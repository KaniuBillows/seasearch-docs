# Deploy

## Download the seasearch.yml

```bash
wget https://haiwen.github.io/seasearch-docs/repo/seasearch.yml
```

## Modify .env file

First, you need to specify the environment variables used by the SeaSearch image in the relevant `.env` file. Some environment variables can be found in [here](../config/README.md). Please add and modify the environment variables (i.e., `<...>`) ​​of the following fields in the `.env` file.

```shell
COMPOSE_FILE='...,seasearch.yml' # ... means other docker-compose yml files

# other environment variables in .env file
# For Apple's chip (M2, e.g.), you should use the images with -nomkl tags (i.e., seafileltd/seasearch-nomkl:latest)
SEASEARCH_IMAGE=seafileltd/seasearch:latest

SS_DATA_PATH=<persistent-volume-path-of-seasearch>
INIT_SS_ADMIN_USER=<admin-username>  
INIT_SS_ADMIN_PASSWORD=<admin-password>
```

## Restart the Service

```shell
docker-compose down
docker-compose up
```

Browse seasearch services in [http://127.0.0.1:4080/](http://127.0.0.1:4080/).
