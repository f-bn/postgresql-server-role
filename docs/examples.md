Examples
--------

#### Manage PostgreSQL host-based access (pg_hba)

```YAML
# Local authentication
postgresql_hba_auth_local: peer         # for local connections via Unix-domain socket
postgresql_hba_auth_host: scram-sha-256 # for local connections via TCP/IP

# Custom entries
postgresql_hba_entries:
- { type: host,  db: 'mydb',        user: 'myuser',      address: '0.0.0.0/0',         method: 'scram-sha-256' }
- { type: host,  db: 'mydb1,mydb2', user: 'mike',        address: '192.168.122.0/24',  method: 'md5' }
- { type: local, db: 'telegraf',    user: 'telegraf',    method: 'peer' }
- { type: host,  db: 'replication', user: 'replication', address: '192.168.122.10/32', method: 'scram-sha-256' }
- { type: host,  db: 'mydb3',       user: 'myuser',      address: '192.168.122.10/32', method: 'ldap', auth_options: 'ldapserver=ldap.example.net ldapbasedn="dc=example, dc=net" ldapsearchfilter="(|(uid=$username)(mail=$username))"' }
- { type: hostssl, db: mydb,        user: 'myuser',      address: '192.168.122.0/24',  method: 'scram-sha-256' }
```

More informations in the official `pg_hba` [documentation](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html).

#### Manage PostgreSQL user name maps (pg_ident)

When using an external authentication system such as Ident or GSSAPI, the name of the operating system user that initiated the connection might not be the same as the database user (role) that is to be used. In this case, a user name map can be applied to map the operating system user name to a database user. 

```YAML
postgresql_ident_entries:
- { mapname: 'omicron', system_username: 'bryanh', pg_username: 'bryanh' }
- { mapname: 'omicron', system_username: 'bryanh', pg_username: 'guest1' }
- { mapname: 'mymap', system_username: '/^(.*)@mydomain\.com$' pg_username: '\1' }
- { mapname: 'mymap', system_username: '/^(.*)@otherdomain\.com$', pg_username: 'guest' }
```

More informations in the official User Name Maps [documentation](https://www.postgresql.org/docs/current/auth-username-maps.html)

#### Manage PostgreSQL synchronous streaming replication parameters

By default, PostgreSQL streaming is asynchronous, but you can configure it to be synchronous if needed :

```YAML
# You need to define a unique name for each replica node
postgresql_cluster_name: "server1"

# Enable synchronous replication (there are multiples options such as 'on', 'remote_write' and 'remote_apply')
postgresql_synchronous_commit: 'on'

# Define the nodes that should be replicated synchronously
postgresql_synchronous_standby_names: "server1, server2, server3"
```

You can also define different topologies for synchronous replication :

```YAML
# This will cause each commit to wait for replies from three higher-priority standbys chosen 
# from standby servers server1, server2, server3 and server4. The standbys whose names appear 
# earlier in the list are given higher priority and will be considered as synchronous.
postgresql_synchronous_standby_names: 'FIRST 3 (server1, server2, server3, server4)'

# This will cause synchronous commit to wait for reply from any 2 standby servers
postgresql_synchronous_standby_names: 'ANY 2 (*)'
```

More informations in the official replication [documentation](https://www.postgresql.org/docs/current/runtime-config-replication.html)

#### Manage SSL/TLS connections

```YAML
postgresql_ssl_enabled: true
postgresql_ssl_ca_file: '/etc/postgresql/root.crt'
postgresql_ssl_cert_file: '/etc/postgresql/server.crt'
postgresql_ssl_key_file: '/etc/postgresql/server.key'
```

Once the variable is defined and the certificates are available for the PostgreSQL server, you can connect to the instance using TLS :

```shell
$ psql -h dbhost.domain.tld -U postgres "sslmode=require sslrootcert=/path/to/root.crt"

psql (15.1 (Ubuntu 15.1-1.pgdg22.04+1))
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, compression: off)
Type "help" for help.

postgres=#
```

More informations in the official SSL/TLS [documentation](https://www.postgresql.org/docs/current/ssl-tcp.html)

#### Manage instance tablespaces

You can manage tablespaces for your database(s) using this role :

```YAML
postgresql_tablespaces:
  - name: tb1
    path: "{{ postgresql_root_dir }}/{{ postgresql_release }}/tb1"
    owner: user                 # optional
    options:                    # optional
      random_page_cost: '1.1'
    state: present              # optional, present by default
  - name: tb2
    path: "{{ postgresql_root_dir }}/{{ postgresql_release }}/tb2"
    state: present
```

More informations in the official tablespace [documentation](https://www.postgresql.org/docs/current/manage-ag-tablespaces.html)

#### Manage instance databases

You can create/drop database using this role :

```YAML
postgresql_databases:
  - name: mydb
    encoding: UTF-8      # optional
    lc_collate: C.UTF-8  # optional
    lc_ctype: C.UTF-8    # optional
    owner: user1         # optional
    conn_limit: 25       # optional
    template: template0  # optional
    tablespace: tb1      # optional
    state: present       # optional, present by default
```

#### Manage instance roles and privileges

You can also create/drop roles with their according privileges with this role :

```YAML
postgresql_roles:
  - name: johndoe
    password: johndoe            # optional
    comment: "The John Doe user" # optional
    expires: infinity            # optional
    role_attr_flags: "LOGIN"     # optional
    conn_limit: 10               # optional
    state: present               # optional, present by default

postgresql_roles_privileges:
  - { type: database, roles: johndoe,  objects: moleculedb, privileges: 'CONNECT' }
```

#### Manage PostgreSQL extensions

You can enable/disable database extensions using this role. Firstly, to use some extensions, you need to install 3rd-party packages. You can use the `postgresql_extra_packages` variable for that :

```YAML
# Here we want to enable at least 'pg_cron' and 'pg_repack', therefore we need to install 2 additional packages

# Debian/Ubuntu
postgresql_extra_packages:
  - "postgresql-{{ postgresql_release }}-cron"
  - "postgresql-{{ postgresql_release }}-repack"

# RHEL
postgresql_extra_packages:
  - "pg_repack_{{ postgresql_release }}"
  - "pg_cron_{{ postgresql_release }}"
```

Then, some extensions requires some libraries to be loaded on server startup, but also some custom settings must be present in the server configuration file :

```YAML
postgresql_shared_preload_libraries:
  - pg_stat_statements
  - pg_prewarm
  - pg_cron

postgresql_extra_configurations:
  - 'pg_stat_statements.max = 10000'
  - 'pg_stat_statements.track = all'
  - 'pg_prewarm.autoprewarm = true'
  - 'pg_prewarm.autoprewarm_interval = 300s'
  - 'cron.use_background_workers = on'
```

Finally, you simple have to choose which extensions you want to enable (and in which database(s) depending on the extension) :

```YAML
postgresql_extensions:
  - name: pg_stat_statements
    databases: ['mydb1','mydb2']
    state: present
  - name: pg_prewarm
    state: present
  - name: pg_buffercache
    state: present
  - name: pg_cron
    state: present
```

#### Use of a dedicated directory/disk for WALs

Some use-cases may require a specific folder to store WALs (i.e on a different disk for performance reasons). You can configure this behavior by simply specifying a folder using the `postgresql_wal_dir` variable :

```YAML
# The folder will be created (with permissions adjusted) and the '--waldir' flag will be passed
# to the initdb command.
postgresql_wal_dir: "{{ postgresql_root_dir }}/{{ postgresql_release }}/wal"
```

[Return to main page](../README.md)
