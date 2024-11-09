# postgresql

## Overview

Installs [PostgreSQL](https://www.postgresql.org/) on OpenBSD.

## Variables

### Required

This role has no required variables. By default, the database cluster is initialized and no users or databases are created.

### Optional

- `postgresql_users`: A list of users to create, defaults to `[]`; for example (assuming `db_user_password` is loaded from an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault) or other secrets manager):
```yaml
- name: db_user
  password: "{{ db_user_password }}"
```
- `postgresql_databases`: A list of databases to create, defaults to `[]`; for example:
```yaml
- name: my_db
  owner: db_user
  encoding: UTF-8
```

## Example

Create an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault) to store your database user password:

```sh
ansible-vault create postgresql_secrets.yml
```

Edit the file to include the password:

```yaml
---
db_user_password: '<password>'
```

Include the role.

```yaml
---
- name: Set up PostgreSQL
  ansible.builtin.include_role:
    name: brianreumere.software.postgresql
  vars_files:
    - postgresql_secrets.yml
  vars:
    postgresql_users:
      - name: db_user
        password: "{{ db_user_password }}"
    postgresql_databases:
      - name: my_db
        owner: db_user
        encoding: UTF-8
```
