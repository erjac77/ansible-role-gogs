# Ansible Role: Gogs

[![Build Status](https://travis-ci.com/erjac77/ansible-role-gogs.svg?branch=master)](https://travis-ci.com/erjac77/ansible-role-gogs)
[![Ansible Quality Score](https://img.shields.io/ansible/quality/14519)](https://galaxy.ansible.com/erjac77/gogs)
[![Ansible Role](https://img.shields.io/ansible/role/14519)](https://galaxy.ansible.com/erjac77/gogs)
[![License](https://img.shields.io/badge/License-Apache%202.0-yellowgreen.svg)](https://opensource.org/licenses/Apache-2.0)

An Ansible role to install Gogs (Go Git Service) on various platforms.

## Requirements

### Supported platforms

* Docker
* Linux

#### Supported distributions

| Distribution | Version            |
| ------------ | ------------------ |
| CentOS       | 6                  |
|              | 7                  |
| Debian       | Buster 10          |
|              | Stretch 9 (stable) |
| Fedora       | 29                 |
|              | 28                 |
| Ubuntu       | Disco 19.04        |
|              | Cosmic 18.10       |
|              | Bionic 18.04 (LTS) |
|              | Xenial 16.04 (LTS) |

## Installation

```
ansible-galaxy install erjac77.gogs
```

## Role Variables

```yaml
# Gogs host platform can be one of: 'Debian', 'RedHat' or 'Docker'
gogs_platform: "{{ ansible_os_family }}"
# gogs_platform: Docker

# Gogs version
gogs_version: 0.11.91

# Download URL for the Gogs binary
gogs_binary_url: "https://github.com/gogits/gogs/releases/download/v{{ gogs_version }}/linux_amd64.zip"

# Gogs dependencies
gogs_dependencies:
  - git
  - unzip

# Gogs run user
gogs_user: git
gogs_user_home: /home/git

# Gogs database settings
gogs_db_type: SQLite3
gogs_db_host: "127.0.0.1:3306"
gogs_db_user: root
gogs_db_pass: default
gogs_db_name: gogs
gogs_db_path: "{{ gogs_user_home }}/gogs/data/gogs.db"

# Gogs application general settings
gogs_app_name: "Gogs: A painless self-hosted Git service"
gogs_repo_root_path: "{{ gogs_user_home }}/gogs-repositories"
gogs_hostname: "{{ hostvars[inventory_hostname]['ansible_default_ipv4']['address'] }}"
gogs_http_port: 3000
gogs_ssh_port: 22
gogs_log_path: "{{ gogs_user_home }}/gogs/log"

# Gogs server and other services settings
gogs_enable_captcha: true

# Gogs connection timeout settings
gogs_connection_retries: 60
gogs_connection_delay: 5

# Gogs admin account settings
gogs_admin_username: gogs
gogs_admin_password: gogs
gogs_admin_fullname: Gogs Administrator
gogs_admin_email: gogs.admin@gogs.io

# Gogs users
gogs_users:
  - username: "{{ gogs_admin_username }}"
    password: "{{ gogs_admin_password }}"
    fullname: "{{ gogs_admin_fullname }}"
    email: "{{ gogs_admin_email }}"
    admin: true

# Gogs organizations
gogs_orgs: []

# Gogs repositories
gogs_repos: []
```

## Dependencies

None.

## Example Playbook

```yaml
- name: Install Gogs on Linux
  hosts: all
  become: true

  vars:
    gogs_platform: Linux
    gogs_enable_captcha: false
    gogs_admin_username: gogs
    gogs_admin_password: gogs
    gogs_admin_fullname: Gogs Administrator
    gogs_admin_email: "gogs.admin@gogs.io"
    gogs_users:
      - username: "{{ gogs_admin_username }}"
        password: "{{ gogs_admin_password }}"
        fullname: "{{ gogs_admin_fullname }}"
        email: "{{ gogs_admin_email }}"
        admin: true
      - username: jodoe
        password: jodo
        fullname: John Doe
        email: "john.doe@gogs.io"
        admin: false
      - username: jadoe
        password: jadoe
        email: "jane.doe@gogs.io"
    gogs_orgs:
      - owner: jodoe
        name: gogs2
        fullname: Gogs2
        description: Gogs(Go Git Service) is a painless self-hosted Git Service.
        website: "https://gogs.io"
        location: USA
    gogs_repos:
      - owner: jodoe
        name: john-awesome-project
        description: My awesome project
        private: true
        auto_init: true
        gitignores: Python
        license: Apache License 2.0
        readme: Default
      - owner: jadoe
        name: jane-cool-project
      - owner: gogs2
        name: gogs2-project

  roles:
    - erjac77.gogs
```

## License

Apache 2.0

## Author Information

Eric Jacob ([@erjac77](https://github.com/erjac77))
