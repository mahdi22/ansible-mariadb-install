![Ansible](https://img.shields.io/ansible/role/d/51033)
![Ansible](https://img.shields.io/ansible/quality/51033)
[![Galaxy](https://img.shields.io/ansible/role/51033)](https://galaxy.ansible.com/mahdi22/mariadb_install)
# Ansible role `mariadb_install`


An Ansible role for installing and secure MariaDB in RHEL/CentOS (7,8) and Debian (9,10) and Ubunut (20.04, 19.10, 18.04, 16.04) distributions. Specifically, the responsibilities of this role are to:

- Install MariaDB packages from the official MariaDB repositories
- Configuration Mariadb server
- Set database root password
- Remove anonmous users
- Remove test database
- Remove remove root remote connection
- Create database
- Create database user
- Import files/*.sql scripts to database

## Installation
``` bash
$ ansible-galaxy install mahdi22.mariadb_install
```

## Role Variables

### Basic configuration

| Variable                       | Default         | Comments                                                                                                     |
| :---                           | :---            | :---                                                                                                         |
| `mariadb_version     `         | '10.4'          | Set mariadb version: - v(10.5 10.4 10.3) RHEL/Centos 8, Debian 10, Ubuntu 20.04, Ubuntu 19.10. - v(10.5 10.4 10.3 10.2 10.1) RHEL/CentOS 7, Debian 9,  Ubuntu 18.04, Ubuntu 16.04                                                                                         |
| `bind-address`                 | '127.0.0.1'     | Set this to the IP address of the network interface to listen on, or '0.0.0.0' to listen on all interfaces.  |
| `configure_swappiness`         | 'True'           | When `True`, this role will set the "swappiness" value (see `mariadb_swappiness`.                            |
| `create_database`                     | 'False'           | Set create_database: True to create database.                                                                                       |
| `database`                     | ''              | Set the database name. Sould be used with create_database: true                                                                                       |
| `port`                         | 3306            | The port number used to listen to client requests                                                            |
| `mysql_root_password`          | 'azerty'              | Set the MariaDB root password                                                                                |
| `mariadb_service`              | mariadb         | Name of the service                                                                                          |
| `swappiness`                   | '10'            | "Swappiness" value (string). System default is 60. A value of 0 means that swapping out processes is avoided.|
| `create_db_user`                     | 'False'           | set create_db_user: True to create user database.                                                                                       |
| `db_user_name`                 | ''      | Set the user to be added. Should be usee with create_database: true, create_db_user: true and database defined                                                                                    |
| `db_user_password`             | ''        | Set the user database password.                                                                              |
| `priviliges`                   | 'ALL'           | Set the user database priviliges.                                                                            |
| `mariadb_logrotate: rotate`    | '7'             | Set mariadb logrotate rotate number.                                                                         |
| `mariadb_logrotate: rotation`  | 'daily'         | Set mariadb logrotate rotation.                                                                              |
| `use_proxy`                 | 'False'      | Set this variable to True if the managed hosts are bihind a web proxy... default False                                                                                    |
| `import_sql_file`                 | 'False'      | set this variable to True to import sql files in databases... default False                                                                                    |
| `sql_file_name`                 | '[]'      | List of sql files name to import in databases (one file or more)... This variable should be defined if import_sql_file is true  |

#### Remarks

(1) To remove remote connection for mysql root user, set the parameter deny_remote_connections: true. E.g.:
defaults/main.yml
```yaml
deny_remote_connections: true
```

(2) **It is highly recommended to set the database root password!** Leaving the password empty is a  security risk. The role will issue a warning if the variable was not set. Default Password "azerty"
defaults/main.yml
```Yaml
mysql_root_password: 'azerty'
```
(3) To import sql script in databases, set import_sql_file: true, define sql_file_name and put your sql scripts in files directory (mahdi22.mariadb_install/files/). E.g.:
defaults/main.yml
```yaml
import_sql_file: true
sql_file_name:
    - sqlscript1.sql
    - sqlscript2.sql
    - sqlscriptN.sql
```
## Example Playbook

Playbook example to execute role using defaults parameters and variables

```Yaml
- hosts: mariadb
  roles:
    - role: mahdi22.mariadb_install
      become: yes
```
Playbook example to execute role using somes variables

```Yaml
- hosts: mariadb
  roles:
    - role: mahdi22.mariadb_install
      become: yes
      vars:
        mariadb_version: "10.5"
        deny_remote_connections: true
        mysql_root_password: "azerty"
        create_database: true
        database: database_test
        create_db_user: true
        db_user_name: user_database
        db_user_password: password
```
Playbook example to execute role using Web Proxy

```Yaml
- hosts: mariadb
  roles:
    - role: mahdi22.mariadb_install
      become: yes
      vars:
        mariadb_version: "10.5"
        deny_remote_connections: true
        mysql_root_password: "azerty"
        create_database: true
        database: database_test
        create_db_user: true
        db_user_name: user_database
        db_user_password: password
        use_proxy: yes
        proxy_env:
          http_proxy: http://proxy.local:8080/
          https_proxy: http://proxy.local:8080/
```

Playbook example to execute role with import sql script

```Yaml
- hosts: mariadb
  roles:
    - role: mahdi22.mariadb_install
      become: yes
      vars:
        mariadb_version: "10.5"
        deny_remote_connections: true
        mysql_root_password: "azerty"
        create_database: true
        database: database_test
        create_db_user: true
        db_user_name: user_database
        db_user_password: password
        use_proxy: yes
        proxy_env:
          http_proxy: http://proxy.local:8080/
          https_proxy: http://proxy.local:8080/
        import_sql_file: true
        sql_file_name:
          - sqlscript1.sql
          - sqlscript2.sql
          - sqlscriptN.sql
```
## Testing

This role is tested on Linux distributions:

- RHEL/CentOS 8
- RHEL/CentOS 7
- Debian 10
- Debian 9
- Debian 8
- Ubuntu 20.04
- Ubuntu 19.10
- Ubuntu 18.04
- Ubuntu 16.04
