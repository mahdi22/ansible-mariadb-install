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
- Import files/table.sql file to database

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
| `configure_swappiness`         | true            | When `true`, this role will set the "swappiness" value (see `mariadb_swappiness`.                            |
| `database`                     | 'cdr'           | set the database name.                                                                                       |
| `port`                         | 3306            | The port number used to listen to client requests                                                            |
| `mysql_root_password`          | ''              | Set the MariaDB root password                                                                                |
| `mariadb_service`              | mariadb         | Name of the service                                                                                          |
| `swappiness`                   | '0'             | "Swappiness" value (string). System default is 60. A value of 0 means that swapping out processes is avoided.|
| `db_user_name`                 | 'userdata'      | Set the user to be added.                                                                                    |
| `db_user_password`             | 'qwerty'        | Set the user database password.                                                                              |
| `priviliges`                   | 'ALL'           | Set the user database priviliges.                                                                            |
| `mariadb_logrotate: rotate`    | '7'             | Set mariadb logrotate rotate number.                                                                         |
| `mariadb_logrotate: rotation`  | 'daily'         | Set mariadb logrotate rotation.                                                                              |

#### Remarks

(1) To remove remote connection for mysql root user, set the parameter allow_remote_connections: true. E.g.:
defaults/main.yml
```yaml
allow_remote_connections: true
```

(2) **It is highly recommended to set the database root password!** Leaving the password empty is a  security risk. The role will issue a warning if the variable was not set.
defaults/main.yml
```Yaml
mysql_root_password: 'azerty'
```


## Example Playbook

```Yaml
- hosts: mariadb
  roles:
    - role: mahdi22.mariadb_install
      become: yes
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
