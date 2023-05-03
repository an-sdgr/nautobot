# Nautobot mvp

## Run locally

```shell-session
docker compose up
```

You can access the webui at `https://localhost:8443/`

Credentials are configured in [local.env](local.env) and default to `admin:admin`

## Getting Started - Plugins

The installation of plugins has a slightly more involved getting started process. See see the [Plugin documentation.](docs/plugins.md).

## Getting Started

1. Have [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/) installed on the host
2. Clone this repository to your Nautobot host into the current user directory.

```shell-session
git clone https://github.com/nautobot/nautobot-docker-compose.git
```

3. Navigate to the new directory from the git clone

```shell-session
cd nautobot-docker-compose
```

4. Copy `local.env.example` to `local.env`

```shell-session
cp local.env.example local.env
```

5. Update the `.env` file for your environment. **THESE SHOULD BE CHANGED** for proper security!

```shell-session
vi local.env
```

6. Update the `.env` to be only available for the current user

```shell-session
chmod 0600 local.env
```

7. Run `docker-compose up` to start the environment

```shell-session
docker-compose up
```

## Super User Account

### Create Super User via Environment

The Docker container has a Docker entrypoint script that allows you to create a super user by the usage of Environment variables. This can be done by updating the example `.env` file environment option of `NAUTOBOT_CREATE_SUPERUSER` to `True`. This will then use the information supplied to create the super user.

### Create Super User via Container

After the containers have started:

1. Verify the containers are running:

```shell-session
docker container ls
```

Example Output:

```shell-session
❯ docker container ls
CONTAINER ID   IMAGE                           COMMAND                  CREATED         STATUS                   PORTS                                                                                  NAMES
143f10daa229   networktocode/nautobot:latest   "nautobot-server rqw…"   2 minutes ago   Up 2 minutes (healthy)                                                                                          nautobot-docker-compose_nautobot-worker_1
bb29124d7acb   networktocode/nautobot:latest   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes (healthy)   0.0.0.0:8080->8080/tcp, :::8080->8080/tcp, 0.0.0.0:8443->8443/tcp, :::8443->8443/tcp   nautobot-docker-compose_nautobot_1
ad57ac1749b3   redis:alpine                    "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes             6379/tcp                                                                               nautobot-docker-compose_redis-queue_1
5ab83264e6fe   postgres:10                     "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes             5432/tcp                                                                               nautobot-docker-compose_postgres_1
a9ec61ce5e30   redis:alpine                    "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes             6379/tcp                                                                               nautobot-docker-compose_redis-cacheops_1
a84a89169300   76e40881ecc6                    "docker-entrypoint.s…"   5 weeks ago     Up 5 hours               5432/tcp                                                                               nautobot_plugin_chatops_ansible_postgres_1
60ef800be813   redis:5-alpine                  "docker-entrypoint.s…"   5 weeks ago     Up 5 hours               6379/tcp                                                                               nautobot_plugin_chatops_ansible_redis_1
```

2. Enter the bash shell for the `nautobot-docker-compose_nautobot_1` container as indicated by the name in the last column for the Nautobot container that has ports listed

```shell-session
docker exec -it nautobot-docker-compose_nautobot_1 bash
```

3. Execute Create Super User Command and follow the prompts

```shell-session
nautobot-server createsuperuser
```

Example Prompts:

```shell-session
nautobot@bb29124d7acb:~$ nautobot-server createsuperuser
Username: administrator
Email address:
Password:
Password (again):
Superuser created successfully.
```
