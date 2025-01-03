# Deploy SeaSearch

This guide provides detailed instructions for deploying SeaSearch, creating user accounts, and utilizing the SeaSearch APIs.

## 1. Download the seasearch.yml

Currently, SeaSearch is **only** deployed through Docker. First, you need to download the corresponding `.yml` and `.env` files. Then you need to modify the configuration information in the `.env` file. Finally, start it through `docker-compose`.

You can download the `.yml` and `.env` files by following commands:

```bash
wget https://haiwen.github.io/seasearch-docs/repo/caddy.yml
wget https://haiwen.github.io/seasearch-docs/repo/seasearch.yml
wget -O .env https://haiwen.github.io/seasearch-docs/repo/env
```

## 2. Modify .env file

First, you need to specify the environment variables used by the SeaSearch image in the downloaded `.env` file. Definition of some variables can be found in [here](../config/README.md). Please modify the environment variables (i.e., `<...>`) ​​of the following fields in the `.env` file.

!!! warning "For Apple's Chips"
    Since Apple's chips (such as M2) do not support [MKL](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl.html), you need to set the relevant image to `seafileltd/seasearch-nomkl:latest` if you use an Apple's chip:

    ```sh
    SEASEARCH_IMAGE=seafileltd/seasearch-nomkl:latest
    ```

```shell
SEASEARCH_SERVER_HOSTNAME=seasearch.example.com

#SEASEARCH_IMAGE=seafileltd/seasearch-nomkl:latest # for Apple's chips
SEASEARCH_IMAGE=seafileltd/seasearch:latest

SS_DATA_PATH=<persistent-volume-path-of-seasearch>
INIT_SS_ADMIN_USER=<admin-username>  
INIT_SS_ADMIN_PASSWORD=<admin-password>
```

## 3. Start the Service

After completing the above steps, you can start the service through the `docker-compose` command:

```shell
docker-compose up -d
```

!!! success "SeaSearch server starts normally"
    After starting the service, you can browse SeaSearch services in `http://seasearch.example.com/` and login by the `INIT_SS_ADMIN_USER` and `INIT_SS_ADMIN_PASSWORD` defined in the `.env` file. 
    
    Finially, you will see the SeaSearch plane like that

    ![grafik](../media/seasearch_console.png)

!!! tip "After first time start SeaSearch Server"
    You can remove the initial admin account informations in `.env` (e.g., `INIT_SS_ADMIN_USER`, `INIT_SS_ADMIN_PASSWORD`), which are only used in the SeaSearch initialization progress (i.e., the **first time** to start services). But make sure **you have recorded it somewhere else in case you forget the password**.
