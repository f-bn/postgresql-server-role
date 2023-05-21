Role Variables
--------------

#### General

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_version`              | `15.3`                       | Defines the package version of PostgreSQL to install              |
| `postgresql_role`                 | `primary`                    | Defines the role of the instance (`primary` or `standby`). This adjust few settings to prepare instance for replication depending on the given role. Set this to `primary` for standalone instance(s) |
| `postgresql_cluster_name`         | `null`                       | Sets a name that identifies this database cluster (instance) for various purposes. The cluster name appears in the process title for all server processes in this cluster. Moreover, it is the default application name for a standby connection |
| `postgresql_shutdown_mode`        | `fast`                       | Defines the shutdown mode to use when stopping instance service. Valid values are `smart` and `fast` |
| `postgresql_restart_on_changes`   | `false`                      | If set to `true`, restart the instance service when server configuration changes |

#### Instance initialization

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_initdb_checksum_enabled`| `false`                      | If set to `true`, enable checksuming on data pages to help detect corruption by the I/O system that would otherwise be silent. Enabling checksums may incur a noticeable performance penalty. |
| `postgresql_initdb_extra_opts`    | `""`                         | Extra args to pass to the `initdb` command                       |

#### Repository and packages

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
|`postgresql_repo_url`              | see [defaults](../defaults/main.yml) | Defines the PostgreSQL packages repository URL           |
|`postgresql_repo_gpgkey_url`       | see [defaults](../defaults/main.yml) | Defines the PostgreSQl packages repository GPG key URL   |
|`postgresql_packages`              | see [defaults](../defaults/main.yml) | Defines the list of packages to install in order to deploy PostgreSQL (this can also be used to install extra packages for extensions) |

#### File locations

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_root_dir`             | `/var/lib/postgresql`        | Defines the root directory where everything related to the PostgreSQL instance is stored |
| `postgresql_home_dir`             | `{{ postgresql_root_dir }}`  | Defines the home directory for the `postgres` user               |
| `postgresql_data_dir`             | `{{ postgresql_root_dir }}/{{ postgresql_release }}/data` | Defines the directory where PostgreSQL will store database data |
| `postgresql_backups_dir`          | `{{ postgresql_root_dir }}/{{ postgresql_release }}/backups` | Defines the directory where PostgreSQL backups are stored |
| `postgresql_config_dir`           | `/etc/postgresql`            | Defines the directory where to store the different PostgreSQL configuration files |

#### Connections and authentications

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_listen_addresses`     | `localhost`                  | Specifies the TCP/IP address(es) on which the server is to listen for connections from client applications. The value takes the form of a comma-separated list of host names and/or numeric IP addresses. |
| `postgresql_port`                 | `5432`                       | The TCP port the server listens on                               |
| `postgresql_max_connections`      | `100`                        | Determines the maximum number of concurrent connections to the database server |
| `postgresql_superuser_reserved_connections` | `3`                | Determines the number of connection “slots” that are reserved for connections by PostgreSQL superusers |
| `postgresql_authentication_timeout` | `1min`                     | Maximum amount of time allowed to complete client authentication |
| `postgresql_password_encryption`  | `scram-sha-256`              | When a password is specified in `CREATE ROLE` or `ALTER ROLE`, this parameter determines the algorithm to use to encrypt the password |
| `postgresql_unix_socket_directories`| `/run/postgresql`          | Specifies the directory of the Unix-domain socket(s) on which the server is to listen for connections from client applications. Multiple sockets can be created by listing multiple directories separated by commas |
| `postgresql_unix_socket_group`    | `-`                          | Sets the owning group of the Unix-domain socket(s). (The owning user of the sockets is always the user that starts the server.) In combination with the parameter `unix_socket_permissions` this can be used as an additional access control mechanism for Unix-domain connections |
| `postgresql_unix_socket_permissions`| `0777`                     | Sets the access permissions of the Unix-domain socket(s). Unix-domain sockets use the usual Unix file system permission set |

#### SSL/TLS

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_ssl_enabled`          | `false`                      | If set to `true`, enable SSL connections to the database         |
| `postgresql_ssl_ca_file`          | `''`                         | Specifies the name of the file containing the SSL server certificate authority (CA) |
| `postgresql_ssl_cert_file`        | `server.crt`                 | Specifies the name of the file containing the SSL server certificate |
| `postgresql_ssl_crl_file`         | `''`                         | Specifies the name of the file containing the SSL client certificate revocation list (CRL) |
| `postgresql_ssl_key_file`         | `server.key`                 | Specifies the name of the file containing the SSL server private key |
| `postgresql_ssl_min_protocol_version`| `TLSv1.2`                 | Specifies the minimum SSL/TLS protocol version to use (`TLSv1`, `TLSv1.1`, `TLSv1.2` or `TLSv1.3`) |

#### Resource usage (except WAL)

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_shared_buffers`       | `128MB`                      | Sets the amount of memory the database server uses for shared memory buffers |
| `postgresql_temp_buffers`         | `8MB`                        | Sets the maximum amount of memory used for temporary buffers within each database session |
| `postgresql_work_mem`             | `4MB`                        | Sets the base maximum amount of memory to be used by a query operation (such as a sort or hash table) before writing to temporary disk files |
| `postgresql_hash_mem_multiplier`  | `1.0`                        | Used to compute the maximum amount of memory that hash-based operations can use |
| `postgresql_maintenance_work_mem` | `64MB`                       | Specifies the maximum amount of memory to be used by maintenance operations, such as `VACUUM`, `CREATE INDEX` and `ALTER TABLE ADD FOREIGN KEY` |
| `postgresql_max_prepared_transactions` | `0`                     | Sets the maximum number of transactions that can be in the `prepared` state simultaneously. Setting this parameter to zero (which is the default) disables the prepared-transaction feature. This parameter can only be set at server start. If you are not planning to use prepared transactions, this parameter should be set to zero to prevent accidental creation of prepared transactions. |
| `postgresql_effective_io_concurrency`| `1`                       | Sets the number of concurrent disk I/O operations that PostgreSQL expects can be executed simultaneously. Raising this value will increase the number of I/O operations that any individual PostgreSQL session attempts to initiate in parallel. The allowed range is `1` to `1000`, or zero to disable issuance of asynchronous I/O requests |
| `postgresql_maintenance_io_concurrency`| `10`                    | Similar to `postgresql_effective_io_concurrency`, but used for maintenance work that is done on behalf of many client sessions |
| `postgresql_max_worker_processes`  | `8`                         | Sets the maximum number of background processes that the system can support |
| `postgresql_max_parallel_workers_per_gather`| `2`                | Sets the maximum number of workers that can be started by a single `Gather` or `Gather Merge` node. |
| `postgresql_max_parallel_maintenance_workers`| `2`               | Sets the maximum number of parallel workers that can be started by a single utility command. Currently, the parallel utility commands that support the use of parallel workers are `CREATE INDEX` only when building a B-tree index, and `VACUUM` without `FULL` option |
| `postgresql_max_parallel_workers` | `8`                          | Sets the maximum number of workers that the system can support for parallel operations |

#### Write-Ahead Log

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_wal_level`            | `replica`                    | Determines how much information is written to the WAL            |
| `postgresql_wal_sync_method`      | `fdatasync`                  | Method used for forcing WAL updates out to disk (see documentation for possible values) |
| `postgresql_wal_compression`      | `off`                        | Defines the compression method used for WAL compression if enabled. Valid values are: `on` or `off` for **PostgreSQL version =< 14.x**. For **PostgreSQL 15.x and later**, new options are available : `pglz`, `lz4` (if PostgreSQL was compiled with `--with-lz4`) or `zstd` (if PostgreSQL was compiled with `--with-zstd`) |
| `postgresql_wal_buffers`          | `-1`                         | The amount of shared memory used for WAL data that has not yet been written to disk. The default setting of `-1` selects a size equal to 1/32nd (about 3%) of `shared_buffers`, but not less than `64kB` nor more than the size of one WAL segment, typically `16MB` |
| `postgresql_synchronous_commit`   | `on`                         | Specifies how much WAL processing must complete before the database server returns a "success" indication to the client. Valid values are `remote_apply`, `on` (the default), `remote_write`, `local`, and `off`.|
| `postgresql_checkpoint_timeout`   | `5min`                       | Maximum time between automatic WAL checkpoints                   |
| `postgresql_recovery_prefetch`    | `try`                        | Whether to try to prefetch blocks that are referenced in the WAL that are not yet in the buffer pool, during recovery. Valid values are `off`, on and `try` (the default). **PostgreSQL 15.x and later required** |
| `postgresql_wal_decode_buffer_size`| `512kB`                     | Defines a limit on how far ahead the server can look in the WAL, to find blocks to prefetch. **PostgreSQL 15.x and later required** |
| `postgresql_archive_mode_enabled` | `false`                      | When `archive_mode` is enabled, completed WAL segments are sent to archive storage by setting `archive_command` |
| `postgresql_archive_library`      | `null`                       | Defines the library to use for archiving completed WAL file segments. If set to an empty string (the default), archiving via shell is enabled, and `archive_command` is used. Otherwise, the specified shared library is used for archiving. **PostgreSQL 15.x and later required** |
| `postgresql_archive_command`      | `null`                       | The local shell command to execute to archive a completed WAL file segment |
| `postgresql_restore_command`      | `null`                       | The local shell command to execute to retrieve an archived segment of the WAL file series. This parameter is required for archive recovery, but optional for streaming replication |

#### Replication

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_max_wal_senders`      | `10`                         | Specifies the maximum number of concurrent connections from standby servers or streaming base backup clients (`primary` node only) |
| `postgresql_max_replication_slots`| `10`                         | Specifies the maximum number of replication slots that the server can support (`primary` node only) |
| `postgresql_synchronous_standby_names`| `""`                     | Specifies a list of standby servers that can support synchronous replication |
| `postgresql_hot_standby`          | `true`                       | Specifies whether or not you can connect and run queries during recovery (`standby` node(s) only ) |

#### Query tuning

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_effective_cache_size` | `4GB`                        | Specifies the planner's assumption about the effective size of the disk cache that is available to a single query |

#### Reporting and logging

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_log_destination`      | `stderr`                     | PostgreSQL logging method (`stderr`, `csvlog` or `syslog`)       |
| `postgresql_logging_collector_enabled` | `false`                 | This parameter enables the logging collector, which is a background process that captures log messages sent to stderr and redirects them into log files |
| `postgresql_log_directory`        | `log`                        | When `logging_collector` is enabled, this parameter determines the directory in which log files will be created |
| `postgresql_log_filename`         | `postgresql-%Y-%m-%d_%H%M%S.log` | When `logging_collector` is enabled, this parameter sets the file names of the created log files |
| `postgresql_log_rotation_age`     | `1d`                         | When `logging_collector` is enabled, this parameter determines the maximum amount of time to use an individual log file, after which a new log file will be created |
| `postgresql_log_min_messages`     | `warning`                    | Controls which message levels are written to the server log (`DEBUG5`, `DEBUG4`, `DEBUG3`, `DEBUG2`, `DEBUG1`, `INFO`, `NOTICE`, `WARNING`, `ERROR`, `LOG`, `FATAL` and `PANIC`) |
| `postgresql_log_min_error_statement` | `error`                   | Controls which SQL statements that cause an error condition are recorded in the server log (`DEBUG5`, `DEBUG4`, `DEBUG3`, `DEBUG2`, `DEBUG1`, `INFO`, `NOTICE`, `WARNING`, `ERROR`, `LOG`, `FATAL` and `PANIC`) |
| `postgresql_log_min_duration_statement` | `-1`                   | Causes the duration of each completed statement to be logged if the statement ran for at least the specified amount of time |
| `postgresql_log_checkpoints`      | `off`                        | Causes checkpoints and restartpoints to be logged in the server log. Some statistics are included in the log messages, including the number of buffers written and the time spent writing them |
| `postgresql_log_connections`      | `off`                        | Causes each attempted connection to the server to be logged, as well as successful completion of both client authentication (if necessary) and authorization |
| `postgresql_log_disconnections`   | `off`                        | Causes session terminations to be logged. The log output provides information similar to `postgresql_log_connections`, plus the duration of the session |
| `postgresql_log_duration`         | `off`                        | Causes the duration of every completed statement to be logged     |
| `postgresql_log_line_prefix`      | `%m [%p] `                   | This is a `printf`-style string that is output at the beginning of each log line. `%` characters begin "escape sequences" that are replaced with status information |
| `postgresql_log_lock_waits`       | `off`                        | Controls whether a log message is produced when a session waits longer than `deadlock_timeout` to acquire a lock |
| `postgresql_log_statement`        | `none`                       | Controls which SQL statements are logged. Valid values are `none` (off), `ddl`, `mod`, and `all` (all statements) |
| `postgresql_log_timezone`         | `Etc/UTC`                    | Sets the time zone used for timestamps written in the server log  |
| `postgresql_syslog_facility`      | `LOCAL0`                     | If `postgresql_log_destination` is set to `syslog`, this parameter determines the syslog facility to be used |
| `postgresql_syslog_ident`         | `postgres`                   | When logging to syslog is enabled, this parameter determines the program name used to identify PostgreSQL messages in syslog logs |
| `postgresql_syslog_sequence_numbers` | `true`                    | If set to `true`, each message will be prefixed by an increasing sequence number |
| `postgresql_syslog_split_messages` | `true`                      | If set to `true`, this parameter determines how messages are delivered to syslog. When `true`, messages are split by lines, and long lines are split so that they will fit into 1024 bytes. When `false`, PostgreSQL server log messages are delivered to the syslog service as is, and it is up to the syslog service to cope with the potentially bulky messages |

#### Statistics

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_track_activities`     | `true`                       | Enables the collection of information on the currently executing command of each session, along with its identifier and the time when that command began execution |
| `postgresql_track_activity_query_size`| `1024`                   | Specifies the amount of memory reserved to store the text of the currently executing command for each active session, for the `pg_stat_activity.query` field |
| `postgresql_track_counts`         | `true`                       | Enables collection of statistics on database activity (required by autovacuum) |
| `postgresql_track_io_timing`      | `false`                      | Enables timing of database I/O calls. This parameter is off by default, as it will repeatedly query the operating system for the current time, which may cause significant overhead on some platforms |
| `postgresql_track_wal_io_timing`  | `false`                      | Enables timing of WAL I/O calls. This parameter is off by default, as it will repeatedly query the operating system for the current time, which may cause significant overhead on some platforms |
| `postgresql_track_functions`      | `none`                       | Enables tracking of function call counts and time used. Specify `pl` to track only procedural-language functions, `all` to also track SQL and C language functions. The default is `none`, which disables function statistics tracking |
| `postgresql_stats_temp_directory` | `pg_stat_tmp`                | Sets the directory to store temporary statistics data in (only available on PostgreSQL version >= 14) |
| `postgresql_compute_query_id`     | `auto`                       | Enables in-core computation of a query identifier. Valid values are `off`, `on`, `auto` and `regress`. |

#### Autovacuum

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_autovacuum_enabled`   | `true`                       | Controls whether the server should run the autovacuum launcher daemon |
| `postgresql_autovacuum_max_workers` | `3`                        | Specifies the maximum number of autovacuum processes (other than the autovacuum launcher) that may be running at any one time |
| `postgresql_autovacuum_naptime`   | `1min`                       | Specifies the minimum delay between autovacuum runs on any given database |

#### Client connection defaults

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_timezone`             | `Etc/UTC`                    | Sets the time zone for displaying and interpreting time stamps   |
| `postgresql_client_encoding`      | `UTF8`                       | Sets the client-side encoding (character set)                    |
| `postgresql_locale`               | `C.UTF-8`                    | Sets the locale                                                  |
| `postgresql_shared_preload_libraries` | `[]`                     | Specifies one or more shared libraries to be preloaded at server start |

#### Lock management

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_deadlock_timeout`     | `1s`                         | This is the amount of time to wait on a lock before checking to see if there is a deadlock condition |
 
#### Error handling

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_restart_after_crash`  | `true`                       | When set to on, which is the default, PostgreSQL will automatically reinitialize after a backend crash |

#### pg_hba

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_hba_auth_local`       | `trust`                      | Specifies the authentication method for local users via Unix-domain socket connections used in `pg_hba.conf` (`local` lines) |
| `postgresql_hba_auth_host`        | `trust`                      | Specifies the authentication method for local users via TCP/IP connections used in `pg_hba.conf` (`host` lines) |
| `postgresql_hba_entries`          | `[]`                         | Entries to set inside the `pg_hba.conf` file to manage Host-Based Access |

#### pg_ident

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_ident_entries`        | `[]`                         | Entries to set inside the `pg_ident.conf` file to manage User Name maps |

#### Extra configurations

| Name                              | Default                      | Description                                                      |
| :-------------------------------- | :--------------------------- | :--------------------------------------------------------------- |
| `postgresql_extra_configurations` | `[]`                         | Defines a list of extra configuration directives to append in the server configuration (i.e extensions settings) |

[Return to main page](../README.md)
