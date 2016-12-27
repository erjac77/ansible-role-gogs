# ANSIBLE ROLE: GOGS

An Ansible role to install Gogs (Go Git Service) on various platforms.

_**Note:** At the moment, Docker is the only platform supported by this role._

## REQUIREMENTS

* Docker >= 1.12.4

## INSTALLATION

#### USING _ANSIBLE GALAXY_:

```
ansible-galaxy install erjac77.gogs
```

#### USING _GIT CLONE_:

Clone this repository inside the `roles/` subdirectory of your playbook or inside one of the additional directories specified by the `roles_path` setting in `ansible.cfg`.

```
git clone https://github.com/erjac77/ansible-role-gogs.git erjac77.gogs
```

## ROLE VARIABLES

```
---

# Gogs host platform
gogs_platform: Docker
#gogs_platform: "{{ ansible_os_family }}"

# Gogs database settings
gogs_db_type: SQLite3
gogs_db_host: 127.0.0.1:3306
gogs_db_user: root
gogs_db_pass: default
gogs_db_name: gogs
gogs_db_path: data/gogs.db

# Gogs application general settings
gogs_app_name: "Gogs: Go Git Service"
gogs_repo_root_path: /data/git/gogs-repositories
gogs_run_user: git
gogs_hostname: "{{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}"
gogs_http_port: 10080
gogs_ssh_port: 10022
gogs_log_path: /app/gogs/log

# Gogs server and other services settings
gogs_enable_captcha: true

# Gogs connection timeout settings
gogs_connection_retries: 60
gogs_connection_delay: 5

# Gogs admin account settings
gogs_admin_username: gogs
gogs_admin_password: gogs
gogs_admin_fullname: Gogs Administrator
gogs_admin_email: gogs.admin@mycompany.com

# Gogs users
gogs_users:
  - { username: "{{ gogs_admin_username }}", password: "{{ gogs_admin_password }}", fullname: "{{ gogs_admin_fullname }}", email: "{{ gogs_admin_email }}", admin: true }

# Gogs repositories
gogs_repos: []
```

### DOCKER VARIABLES

```
---

# Gogs Docker container settings
gogs_container_name: gogs
gogs_container_image: gogs/gogs
gogs_container_http_host_port: "{{ gogs_http_port }}"
gogs_container_http_container_port: 3000
gogs_container_ssh_host_port: "{{ gogs_ssh_port }}"
gogs_container_ssh_container_port: 22
gogs_container_data_volume: gogs-data
```

## DEPENDENCIES

* [ansible-role-docker](https://github.com/erjac77/ansible-role-docker)

## EXAMPLE PLAYBOOK

```
---

- name: Install Gogs on Docker
  hosts: localhost
  become: yes

  vars:
    gogs_platform: Docker
    gogs_enable_captcha: false
    gogs_admin_username: erjac
    gogs_admin_password: erjac
    gogs_admin_fullname: Eric Jacob
    gogs_admin_email: erjac77@gmail.com
    gogs_users:
      - { username: "{{ gogs_admin_username }}", password: "{{ gogs_admin_password }}", fullname: "{{ gogs_admin_fullname }}", email: "{{ gogs_admin_email }}", admin: true }
      - { username: jodoe, password: jodoe, fullname: John Doe, email: john.doe@mycompany.com, admin: false }
      - { username: jadoe, password: jadoe, email: jane.doe@mycompany.com }
    gogs_repos:
      - { owner: jodoe, name: john-awesome-project, description: My awesome project, private: true, auto_init: true, gitignores: "Python", license: "Apache License 2.0", readme: "Default" }
      - { owner: jadoe, name: jane-cool-project }

  roles:
    - erjac77.gogs
```

## LICENSE

Apache 2.0

## AUTHOR INFORMATION

Eric Jacob ([@erjac77](https://github.com/erjac77))