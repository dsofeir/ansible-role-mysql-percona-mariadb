# Ansible role: MySQL/Percona/MariaDB Server

An ansible role which initialises MySQL/Percona/MariaDB Server software on a host

## Role Variables

The following variables can be set via parameters to the role.

    mysql_percona_mariadb:
      vendor: mariadb
      root_password: " {{ strong_password_from_the_vault }}"
      custom_config: ~
      users: []
      databases: []

Parameter | Choices/_Defaults_ | Comments
--- | --- | ---
mysql_percona_mariadb.vendor (_string_, optional) | _mariadb_: distribution provided MariaDB package; mysql: mysql community package | Select what server software is installed
mysql_percona_mariadb.root_password (_string_, **required**) | | Password for root user account. It is reccommended that this be stored in the Ansible Vault
mysql_percona_mariadb.custom_config (_string_, optional) | | Custom configuration which is appended to `/etc/my.cnf`
mysql_percona_mariadb.database (_list_, optional) | | Custom configuration which is appended to `/etc/my.cnf`
mysql_percona_mariadb.users (_hash_, optional) | | Hash containing users to be created of the form: `{ username: 'example_user_1', password: "{{ strong_password_from_the_vault_2 }}", host: 'localhost', privileges: 'ALL', db: 'example_database_1.*' }`

### Important Notes

* Passwords are passed as parameters to the `mysql` and `mysqladmin` command on the server. The `command` Ansible module is used so there should be not reference in shell logs, however this is not the most secure method. Unfortunately it must be done this way for compatability.
* The root password will only be set once. Changing the root password after it is initialy set will not update the password on the server
* Users will only be created and have their password set once. Changing a user's password after it is initialy created will not update the password on the server
* Removing users will not result in their deletion on the server
* Removing databases will not result in their deletion on the server

## Example Playbook

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      become: yes

      roles:
        - role: dsofeir.mysql_percona_mariadb
          vars:
            mysql_percona_mariadb:
              vendor: mariadb
              root_password: "{{ strong_password_from_the_vault_0 }}"
              databases:
                - example_database_0
                - example_database_1
              users:
                - { username: 'example_user_0', password: "{{ strong_password_from_the_vault_1 }}", host: 'localhost', privileges: 'ALL', db: 'example_database_0.*' }
                - { username: 'example_user_1', password: "{{ strong_password_from_the_vault_2 }}", host: 'localhost', privileges: 'ALL', db: 'example_database_1.*' }
              custom_config: |
                              log-bin
                              server_id=1
                              log-basename=example_server_com
                              expire_logs_days=7

## Todo

* Support for Percona Server
* Support for Ubuntu Linux

## License

Apache 2.0 -  http://www.apache.org/licenses/LICENSE-2.0

## Author Information

David Foley, dev@dfoley.ie, https://www.dfoley.ie
