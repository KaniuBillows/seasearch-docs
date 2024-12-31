# Configure Seafile Pro to use SeaSearch 

## Stop Seafile

=== "Deploy with Docker"

    !!! warning "Important note for Seafile Pro Single Node User"
        By default, Seafile Docker will use *Elasticsearch* as the indexer. You **have to disable** *Elasticsearch* before you change to use SeaSearch, which just need to remove whole section about the service `elasticsearch` in `seafile-server.yml`:

        ```yml
        services:
            ...

            elasticsearch: # remove this section
                ... # and its contents

        ```

    ```sh
    docker compose down
    ```

=== "Deploy from binary packages"
    !!! note
        For installations using python virtual environment, activate it if it isn't already active

        ```sh
        source python-venv/bin/activate
        ```

    === "Single node"

        ```sh
        cd /opt/seafile/seafile-server-latest
        su seafile
        ./seafile.sh stop
        ./seahub.sh stop
        ```

    === "cluster"

        ```sh
        cd /opt/seafile/seafile-server-latest
        su seafile
        ./seafile-background-tasks.sh stop # just stop backend task is fine
        ```

## Modify seafevents.conf

update `es_host` and `es_port` to the SeaSearch server's host (`seasearch` if the same host deployment with Seafile) and port (`4080` by default)

```conf
es_host = seasearch
es_port = 4080
```

## Restart Server

=== "Deploy with Docker"

    ```sh
    docker compose up
    ```

=== "Deploy from binary packages"
    !!! note
        For installations using python virtual environment, activate it if it isn't already active

        ```sh
        source python-venv/bin/activate
        ```

    === "Single node"

        ```sh
        cd /opt/seafile/seafile-server-latest
        su seafile
        ./seafile.sh start
        ./seahub.sh start
        ```

    === "cluster"

        ```sh
        cd /opt/seafile/seafile-server-latest
        su seafile
        ./seafile-background-tasks.sh start # just start backend task is fine
        ```
