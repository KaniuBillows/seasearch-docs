# Deploy

SeaSearch is now only supported deploying with Docker. You can follow this document to deploy SeaSearch, initial administrator, create regular user and use SeaSearch APIs

## 1. Download the seasearch.yml

```bash
wget https://haiwen.github.io/seasearch-docs/repo/seasearch.yml
```

## 2. Modify .env file

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

## 3. Restart the Service

```shell
docker-compose down
docker-compose up
```

!!! success "You can browse SeaSearch services in [http://127.0.0.1:4080/](http://127.0.0.1:4080/)"

## 4. Access API to Create regular user via admin account

### Get auth token:

!!! note
    SeaSearch's auth token is using **base64 encode** consist of `username` and `password`, you can check [here](../api/authentication.md) for the whole details

```sh
echo -n 'aladdin:opensesame' | base64
YWxhZGRpbjpvcGVuc2VzYW1l
```

You can use your auth token to access SeaSearch APIs by adding it into `headers`, i.e.,

```json
headers = {
    "Authorization": "Basic YWxhZGRpbjpvcGVuc2VzYW1l"
}
```

### Create a regular user

!!! tip 
    We here just show an example to describe how to use auth token to access SeaSearch APIs, you can check [here](../api/overview.md) for the whole details of SeaSearch APIS.

You can create a regular user by **POST `/api/user`**, e.g.,

```sh
curl -X POST http://127.0.0.1:4080/api/user \
-H "Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l" \
-H "Content-Type: application/json" \
-d '{
    "_id": "prabhat",
    "name": "newusername",
    "role": "user",
    "password": "Complexpass#123"
}'
```

!!! success
    After submitting a **POST** to `/api/user`, you will recive a response

    ```json
    {
        "message": "ok",
        "id": "prabhat"
    }
    ```

    Then, you can remove the initial admin account informations in `.env` (i.e., `INIT_SS_ADMIN_USER`, `INIT_SS_ADMIN_PASSWORD`)

## 5. Access SeaSearch APIs via regular user

### Get auth token

```sh
echo -n 'newusername:Complexpass#123' | base64
bmV3dXNlcm5hbWU6Q29tcGxleHBhc3MjMTIz
```

### Create an index

You can create an index by **PUT `/Index_name`** with related settings, for exmaple:

```sh
curl -X POST http://127.0.0.1:4080/my-index-000001 \
-H "Authorization: Basic YWxhZGRpbjpvcGVuc2VzYW1l" \
-H "Content-Type: application/json" \
-d '{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 2
  }
}'
```

response

```json
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "logs"
}
```
