# Deploy

## Download the seasearch.yml

```bash
wget https://manual.seafile.com/12.0/docker/pro/seasearch.yml
```

## Modify .env file

First, you need to specify the environment variables used by the SeaSearch image in the relevant `.env` file. Some environment variables can be found in [here](../config/README.md). Please add and modify the environment variables (i.e., `<...>`) ​​of the following fields in the `.env` file.

```shell
COMPOSE_FILE='...,seasearch.yml' # ... means other docker-compose yml

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

!!! success "You can browse SeaSearch services in [http://127.0.0.1:4080/](http://127.0.0.1:4080/)"

!!! danger "Important note"
    By default, the SeaSearch server **will accept all connection** by listening `4080` port. We suggest you to set firewall to set and enable only the Seafile server can connect:

    === "SeaSearch is deployed on the same machine as Seafile"
        1. Remove the exposed ports in the `seasearch.yml`

        2. Set `es_host` to `seasearch` in `seafevents.conf`

    === "SeaSearch is deployed on a different machine with Seafile"

        ```sh
        sudo iptables -A INPUT -p tcp -s <your seafile server host> --dport 4080 -j ACCEPT
        sudo iptables -A INPUT -p tcp --dport 4080 -j DROP
        ```
