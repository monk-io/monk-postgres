# PostgreSQL & Monk

This repository contains a Monk.io template to deploy a PostgreSQL system, either locally or on the cloud provider of your choice (AWS, GCP, Azure, Digital Ocean).

## Prerequisites

- [Install Monk](https://docs.monk.io/docs/get-monk)
- [Register and Login to Monk](https://docs.monk.io/docs/acc-and-auth)
- [Add a Cloud Provider](https://docs.monk.io/docs/cloud-provider)
- [Add an Instance](https://docs.monk.io/docs/multi-cloud)

## Ensure `monkd` is Running

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
cd monk-postgresql/postgres-extended
monk load MANIFEST
```

## Verify Loaded Templates

To see a list of the loaded templates, use:

```bash
foo@bar:~$ monk list postgresql-extended
✔ Got the list
Type      Template       Repository  Version  Tags
runnable  postgresql-extended/db  local       -        -
```

## Deploy Stack

Run the following command to deploy the PostgreSQL stack:

```bash
foo@bar:~$ monk run postgresql-extended/db
✔ Starting the job: local/postgresql/db... DONE
✔ Preparing nodes DONE
✔ Checking/pulling images DONE
✔ Started local/postgresql-extended/db

🔩 templates/local/postgresql-extended/db
 └─🧊 Peer monk-3
    └─🔩 templates/local/postgresql-extended/db
       └─📦 4020138f7583d57965317076d49641a0-al-monk-postgresql-extended-db-postgres
          ├─🧩 postgres:12.2
          ├─💾 /var/lib/monkd/volumes/postgres/db1 -> /var/lib/postgresql/data
          └─🔌 open 16.171.22.57:5435 (0.0.0.0:5435) -> 5432

💡 You can inspect and manage your above stack with these commands:
 monk logs (-f) local/postgresql-extended/db - Inspect logs
 monk shell     local/postgresql-extended/db - Connect to the container's shell
 monk do        local/postgresql-extended/db/action_name - Run defined action (if exists)
💡 Check monk help for more!
```

## Variables

The variables are defined in the stack section of postgresql.yml. You can quickly set them up by editing the values here.

| Variable                      | Description                                        | Default                                  |
|-------------------------------|----------------------------------------------------|------------------------------------------|
| db_user                       | Database user                                      | monk                                     |
| db_pass                       | Database password                                  | adminpassword                            |
| db_name                       | Database name                                      | monk                                     |
| volume_dir                    | Base directory for persistence                     | /bitnami/postgresql                      |
| data_dir                      | PostgreSQL data directory                          | ${volume_dir}/data                       |
| allow_remote_connections      | Allow remote connections to the database           | yes                                      |
| replication_mode              | Set replication mode (master, slave)               | master                                   |
| replication_user              | Replication user                                   | repl_user                                |
| replication_password          | Password for replication user                      | repl_password                            |
| max_connections               | Maximum connections allowed                        | 100                                      |
| timezone                      | Set the timezone for the database                  | UTC                                      |
| shared_preload_libraries      | Libraries to preload on startup                    | pgaudit                                  |
| wal_level                     | Set the Write-Ahead Logging level                  | replica                                  |
| fsync                         | Enable fsync for write-ahead logs                  | on                                       |
| synchronous_commit_mode       | Enable synchronous replication                     | on                                       |
| log_connections               | Log each user connection                           | yes                                      |
| log_disconnections            | Log each user disconnection                        | yes                                      |
| extra_flags                   | Extra flags for PostgreSQL initialization          | nil                                      |
| init_max_timeout              | Maximum initialization waiting timeout             | 60                                       |
| pgctl_timeout                 | Maximum waiting timeout for pg_ctl commands        | 60                                       |
| shutdown_mode                 | Default mode for pg_ctl stop command               | fast                                     |
| cluster_app_name              | Replication cluster default application name       | walreceiver                              |
| initdb_args                   | Optional args for PostgreSQL initdb operation      | nil                                      |
| initdb_wal_dir                | Optional init db wal directory                     | nil                                      |
| master_host                   | PostgreSQL master host (used by slaves)            | nil                                      |
| master_port_number            | PostgreSQL master host port (used by slaves)       | 5432                                     |
| num_synchronous_replicas      | Number of PostgreSQL replicas for sync replication | 0                                        |
| synchronous_replicas_mode     | PostgreSQL synchronous replication mode            | nil                                      |
| port_number                   | PostgreSQL port number                             | 5432                                     |
| ldap_enable                   | Enable LDAP for PostgreSQL authentication          | no                                       |
| ldap_url                      | PostgreSQL LDAP server URL                         | ""                                       |
| ldap_base_dn                  | PostgreSQL LDAP base DN                            | ""                                       |
| tls_enable                    | Enable TLS for database traffic                    | no                                       |
| tls_cert_file                 | File containing TLS certificate                    | /path/to/tls_cert_file                   |
| tls_key_file                  | File containing TLS key                            | /path/to/tls_key_file                    |
| client_min_messages           | Set log level of errors for client                 | error                                    |
| statement_timeout             | SQL statement timeout                              | 30000                                    |
| tls_ca_file                   | File containing TLS CA certificate                 | ""                                       |
| tls_crl_file                  | File containing TLS Certificate Revocation List    | ""                                       |
| tls_prefer_server_ciphers     | Prefer server TLS cipher preferences               | yes                                      |
| pgaudit_log                   | Comma-separated actions to log with pgaudit        | nil                                      |
| pgaudit_log_catalog           | Enable pgaudit log catalog                         | nil                                      |
| pgaudit_log_parameter         | Enable pgaudit log parameter                       | nil                                      |
| log_hostname                  | Log client host name                               | nil                                      |
| log_line_prefix               | Format of log lines                                | nil                                      |
| log_timezone                  | Log timezone                                       | nil                                      |
| tcp_keepalives_idle           | TCP keepalive idle time                            | nil                                      |
| tcp_keepalives_interval       | TCP keepalive interval time                        | nil                                      |
| tcp_keepalives_count          | TCP keepalive count                                | nil                                      |
| pghba_remove_filters          | Strings to remove from pg_hba.conf                 | nil                                      |
| username_connection_limit     | User connection limit                              | nil                                      |
| postgres_connection_limit     | postgres user connection limit                     | nil                                      |
| default_toast_compression     | PostgreSQL default compression                     | nil                                      |
| password_encryption           | Password encryption method                         | nil                                      |
| default_transaction_isolation | Default transaction isolation                      | nil                                      |
| autoctl_conf_dir              | Path to pg_autoctl configuration directory         | ${POSTGRESQL_AUTOCTL_VOLUME_DIR}/.config |
| autoctl_mode                  | pgAutoFailover node type                           | postgres                                 |
| autoctl_monitor_host          | Hostname for monitor component                     | monitor                                  |
| autoctl_hostname              | Hostname for PostgreSQL access                     | $(hostname --fqdn)                       |

## Stop, remove and clean up workloads and templates

To stop and clean up the PostgreSQL stack:

```bash
monk purge -a postgresql-extended/db
```

