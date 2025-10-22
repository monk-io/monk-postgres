# Postgresql Leader Follower & Monk

This repository contains Monk.io template to deploy Postgresql Leader Follower cluster either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

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
cd monk-postgresql/postgresql-cluster-pgpool
monk load MANIFEST
```

## Let's take a look at the themes I have installed

```bash
foo@bar:~$ monk list postgresql-cluster-pgpool
✔ Got the list
Type      Template                               Repository  Version  Tags
runnable  postgresql-cluster-pgpool/db1     local       -        -
runnable  postgresql-cluster-pgpool/db2     local       -        -
runnable  postgresql-cluster-pgpool/dbpool  local       -        -
group     postgresql-cluster-pgpool/stack   local       -        -

```

## Deploy Stack

```bash
foo@bar:~$ monk run postgresql-cluster-pgpool/stack
? Select tag to run [local/postgresql-cluster-pgpool/stack] on: postgres
✔ Starting the job: local/postgresql-cluster-pgpool/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% docker.io/bitnami/postgresql-repmgr:14 db-2
✔ [================================================] 100% docker.io/bitnami/postgresql-repmgr:14 db-3
✔ [================================================] 100% docker.io/bitnami/pgpool:4 db-1
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Starting containers DONE
✔ Started local/postgresql-cluster-pgpool/stack

🔩 templates/local/postgresql-cluster-pgpool/stack
 ├─🧊 Peer db-1
 │  └─🔩 templates/local/postgresql-cluster-pgpool/dbpool
 │     └─📦 8309ac085fb3fe18be0793bb166a0375-l-cluster-pgpool-dbpool-pgpool
 │        ├─🧩 docker.io/bitnami/pgpool:4
 │        └─🔌 open 13.50.15.241:5440 -> 5432
 ├─🧊 Peer db-3
 │  └─🔩 templates/local/postgresql-cluster-pgpool/db2
 │     └─📦 94d61b60bab748ad44be532c4b2338cc-ql-cluster-pgpool-db2-monk-db2
 │        ├─🧩 docker.io/bitnami/postgresql-repmgr:14
 │        ├─💾 /var/lib/monkd/volumes/postgres/db2 -> /bitnami/postgresql
 │        └─🔌 open 16.170.222.245:5436 (0.0.0.0:5436) -> 5432
 └─🧊 Peer db-2
    └─🔩 templates/local/postgresql-cluster-pgpool/db1
       └─📦 1df4dde31d6943e0a4dd8ce576742e3a-ql-cluster-pgpool-db1-monk-db1
          ├─🧩 docker.io/bitnami/postgresql-repmgr:14
          ├─💾 /var/lib/monkd/volumes/postgres/db1 -> /bitnami/postgresql
          └─🔌 open 16.170.37.16:5435 (0.0.0.0:5435) -> 5432

💡 You can inspect and manage your above stack with these commands:
 monk logs (-f) local/postgresql-cluster-pgpool/stack - Inspect logs
 monk shell     local/postgresql-cluster-pgpool/stack - Connect to the container's shell
 monk do        local/postgresql-cluster-pgpool/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are in `stack.yml` file. You can quickly setup by editing the values here.

| Variable         | Description                                | Defaults |
| ---------------- | ------------------------------------------ | -------- |
| postgres_version | Porsgresql image version, docker image tag | 14       |
| pgpool_image_tag | Pgpool image version                       | 4        |
| db_user          | Db user                                    | monk     |
| db_pass          | Db password                                | monk     |
| db_name          | Database name                              | monk     |

## Stop, remove and clean up workloads and templates

```bash
monk purge -x postgresql-cluster-pgpool/stack postgresql-cluster-pgpool/db1 postgresql-cluster-pgpool/db2 postgresql-cluster-pgpool/dbpool
```
