<p><img src="https://icon-library.com/images/postgresql-icon/postgresql-icon-20.jpg" alt="pg-logo" title="pg" align="top" height=165 /></p>

*PostgreSQL is a powerful, open source object-relational database system that uses and extends the SQL language combined with many features that safely store and scale the most complicated data workloads. PostgreSQL has earned a strong reputation for its proven architecture, reliability, data integrity, robust feature set, extensibility, and the dedication of the open source community behind the software to consistently deliver performant and innovative solutions.*

### General informations

This Ansible role is designed to deploy and configure standalone PostgreSQL instance(s) on target servers.

**Table of Contents**

  - [Roles variables](#role-variables)
  - [Roles tags](#role-variables)
  - [Install and use this role](#install-and-use-this-role)

**Supported Platforms**

  - Ubuntu 22.04 *Jammy Jellyfish*
  - Debian 11 *Bullseye*
  - Red Hat Enterprise Linux 8.x/9.x

**Supported PostgreSQL releases**

  - PostgreSQL 15.x - **default**
  - PostgreSQL 14.x

**Requirements**

  - `community.postgresql` collection (tested with version `2.4.1`)

**References**

  - PostgreSQL : https://www.postgresql.org/

### Role variables

The role variables documentation are available here :

  - [General](docs/variables.md#general)
  - [Instance initialization](docs/variables.md#instance-initialization)
  - [File locations](docs/variables.md#file-locations)
  - [Connections and authentication](docs/variables.md#connections-and-authentications)
  - [SSL/TLS](docs/variables.md#ssltls)
  - [Resource Usage](docs/variables.md#resource-usage-except-wal)
  - [Write-Ahead Log](docs/variables.md#write-ahead-log)
  - [Replication](docs/variables.md#replication)
  - [Reporting and logging](docs/variables.md#reporting-and-logging)
  - [Statistics](docs/variables.md#statistics)
  - [Autovacuum](docs/variables.md#autovacuum)
  - [Client connection defaults](docs/variables.md#client-connection-defaults)
  - [Lock management](docs/variables.md#lock-management)
  - [Error handling](docs/variables.md#error-handling)
  - [pg_hba](docs/variables.md#pg_hba)
  - [pg_ident](docs/variables.md#pg_ident)
  - [Tablespaces](docs/variables.md#tablespaces)
  - [Databases](docs/variables.md#databases)
  - [Roles and privileges](docs/variables.md#roles-and-privileges)
  - [Extensions](docs/variables.md#extensions)
  - [Miscellaneous](docs/variables.md#miscellaneous)

### Examples

You can find some configurations examples :

- [Manage PostgreSQL host-based access (pg_hba)](docs/examples.md#manage-postgresql-host-based-access-pg_hba)
- [Manage PostgreSQL user name maps (pg_ident)](docs/examples.md#manage-postgresql-user-name-maps-pg_ident)
- [Manage PostgreSQL synchronous streaming replication parameters](docs/examples.md#manage-postgresql-synchronous-streaming-replication-parameters)
- [Manage SSL/TLS connections](docs/examples.md#manage-ssltls-connections)
- [Manage instance tablespaces](docs/examples.md#manage-instance-tablespaces)
- [Manage instance databases](docs/examples.md#manage-instance-databases)
- [Manage instance roles and privileges](docs/examples.md#manage-instance-roles-and-privileges)
- [Manage PostgreSQL extensions](docs/examples.md#manage-postgresql-extensions)
- [Use of a dedicated directory/disk for WALs](docs/examples.md#use-of-a-dedicated-directorydisk-for-wals)

### Install and use this role

* Install the role using the command-line :

  ```shell
  $ ansible-galaxy role install git+https://github.com/f-bn/postgresql-server-role.git
  ```

* You can also install the role in your projects using a `requirements.yml` file and `ansible-galaxy` command-line :

  ```YAML
  $ cat requirements.yml
  ---
  roles:
    - name: postgresql_server
      src: https://github.com/f-bn/postgresql-server-role.git
      scm: git
      version: '1.0.0'

  $ ansible-galaxy install-f -r requirements.yml
  ```

* Once the role is installed, you can use it in your playbooks :

  ```yaml
  - name: Install PostgreSQL server
    hosts: postgresql
    roles:
      - role: postgresql_server
  ```

### Roadmap

- [x] Roles management
- [x] Databases management
- [x] Extensions management
- [X] Tablespaces management
