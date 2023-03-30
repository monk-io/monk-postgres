# Postgresql & Monk

This repository contains Monk.io template to deploy Postgresql system either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

## Make sure monkd is running

```bash
foo@bar:~$ monk status
daemon: ready
auth: logged in
not connected to cluster
```

## Clone Repository

```bash
git clone https://github.com/monk-io/monk-postgresql
```

## Load Template

```bash
cd monk-postgresql/postgresql
monk load MANIFEST
```

## Let's take a look at the themes I have installed

```bash
foo@bar:~$ monk list postgresql
✔ Got the list
Type      Template            Repository  Version  Tags
runnable  postgresql/db  local       -        -
```

## Deploy Stack

```bash
foo@bar:~$ monk run postgresql/db
✔ Starting the job: local/postgresql/db... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images DONE
✔ Started local/postgresql/db

🔩 templates/local/postgresql/db
 └─🧊 Peer monk-3
    └─🔩 templates/local/postgresql/db
       └─📦 4020138f7583d57965317076d49641a0-al-monk-postgresql-db-postgres
          ├─🧩 postgres:12.2
          ├─💾 /var/lib/monkd/volumes/postgres/db1 -> /var/lib/postgresql/data
          └─🔌 open 16.171.22.57:5435 (0.0.0.0:5435) -> 5432

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/postgresql/db - Inspect logs
	monk shell     local/postgresql/db - Connect to the container's shell
	monk do        local/postgresql/db/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are stack section in `redis.yml` file. You can quickly setup by editing the values here.

| Variable          | Description                  | Default                      |
| ----------------- | ---------------------------- | ---------------------------- |
| image_tag         | Docker image tag             | 12.2                         |
| postgres_password | Postgresql Password          | adminpassword                |
| db_username       | Postgresql user name         | monk                         |
| db_name           | postgresql db name           | monk                         |
| volume_data       | Postgresql local volume path | ${monk-volume-path}/postgres |

## Stop, remove and clean up workloads and templates

```bash
monk purge -a postgresql/db
```
