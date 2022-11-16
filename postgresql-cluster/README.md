# Postgresql Master Slave & Monk
This repository contains Monk.io template to deploy Postgresql Master Slave cluster either locally or on cloud of your choice (AWS, GCP, Azure, Digital Ocean).

# Prerequisites
- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add Instance](https://docs.monk.io/docs/multi-cloud)

#### Make sure monkd is running.
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
cd postgresql-cluster
monk load MANIFEST
```


#### Let's take a look at the themes I have installed.
```bash
foo@bar:~$ monk list monk-postgresql-cluster
✔ Got the list
Type      Template                               Repository  Version  Tags
runnable  monk-postgresql-cluster/db1     local       -        -
runnable  monk-postgresql-cluster/db2     local       -        -
group     monk-postgresql-cluster/stack   local       -        -


```

## Deploy Stack
```bash
foo@bar:~$ monk run monk-postgresql-cluster/stack
? Select tag to run [local/monk-postgresql-cluster/stack] on: postgres
✔ Starting the job: local/monk-postgresql-cluster/stack... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images...
✔ [================================================] 100% docker.io/bitnami/postgresql-repmgr:14 db-2
✔ [================================================] 100% docker.io/bitnami/postgresql-repmgr:14 db-3
✔ Checking/pulling images DONE
✔ Starting containers DONE
✔ Starting containers DONE

🔩 templates/local/monk-postgresql-cluster-pgpool/stack
 ├─🧊 Peer db-1
 │  └─🔩 templates/local/monk-postgresql-cluster-pgpool/db2
 │     └─📦 94d61b60bab748ad44be532c4b2338cc-ql-cluster-pgpool-db2-monk-db2
 │        ├─🧩 docker.io/bitnami/postgresql-repmgr:14
 │        ├─💾 /var/lib/monkd/volumes/postgres/db2 -> /bitnami/postgresql
 │        └─🔌 open 13.50.15.241:5436 (0.0.0.0:5436) -> 5432
 ├─🧊 Peer db-3
 │  └─🔩 templates/local/monk-postgresql-cluster-pgpool/db1
 │     └─📦 1df4dde31d6943e0a4dd8ce576742e3a-ql-cluster-pgpool-db1-monk-db1
 │        ├─🧩 docker.io/bitnami/postgresql-repmgr:14
 │        ├─💾 /var/lib/monkd/volumes/postgres/db1 -> /bitnami/postgresql
 │        └─🔌 open 16.170.222.245:5435 (0.0.0.0:5435) -> 5432
 └─🧊 Peer db-2
    ├─🔩 templates/local/monk-postgresql-cluster-pgpool/dbpool
    │  └─📦 16157a94ba45b83b8bd9f035a4bc0df2-ster-pgpool-dbpool-monk-alpine
    │     └─🧩 docker.io/library/alpine:latest
    └─🔩 templates/local/monk-postgresql-cluster-pgpool/dbpool
       └─📦 8309ac085fb3fe18be0793bb166a0375-l-cluster-pgpool-dbpool-pgpool
          ├─🧩 docker.io/bitnami/pgpool:4
          └─🔌 open 16.170.37.16:5440 -> 5432

💡 You can inspect and manage your above stack with these commands:
	monk logs (-f) local/monk-postgresql-cluster-pgpool/stack - Inspect logs
	monk shell     local/monk-postgresql-cluster-pgpool/stack - Connect to the container's shell
	monk do        local/monk-postgresql-cluster-pgpool/stack/action_name - Run defined action (if exists)
💡 Check monk help for more!
```


## Variables
The variables are in `stack.yml` file. You can quickly setup by editing the values here.

| Variable                     	| Description                               	|
|------------------------------	|-------------------------------------------	|
| postgresql_image_tag          	| Porsgresql image version 	               |
| pgpool_image_tag             	| Pgpool image version                      	|
| db_postgres_password        	| Postgres User's password 	|
| db_postgresql_username         | Db user           	|
| db_postgresql_password        	| Db password         	|
| db_postgresql_name          	| Database name 	|
| db_replication_password        | Replication password     	|
| db_replication_port          	| Replication port     	|


## 

## Stop, remove and clean up workloads and templates

```bash
monk purge -x monk-postgresql-cluster-pgpool/stack monk-postgresql-cluster-pgpool/db1 monk-postgresql-cluster-pgpool/db2
```