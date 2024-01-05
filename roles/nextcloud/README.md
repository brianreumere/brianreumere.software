# nextcloud

## Overview

Installs [Nextcloud](https://docs.nextcloud.com/server/latest/admin_manual/) on RHEL (with Apache, PHP 8.x, and MySQL). If the webroot already exists on subsequent runs, the role skips installation steps and assumes there is an existing configured installation.

Since SELinux is enabled, to perform future upgrades via the web interface first run:

```
setsebool httpd_unified on
```

When the upgrade is complete, run:

```
setsebool -P httpd_unified  off
```

## Variables

### Required

- `nextcloud_hostname`: The hostname where Nextcloud will be available

### Secrets

These are required and should be set in an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault) or sourced from another secrets manager. See the example below.

- `nextcloud_mysql_password`: MySQL password for the Nextcloud user
- `nextcloud_instance_id`: ?
- `nextcloud_redis_password`: Redis password
- `nextcloud_admin_user`: Initial Nextcloud admin user to create
- `nextcloud_admin_pass`: Password for the initial Nextcloud admin user

### Optional

- `nextcloud_version`: The version of Nextcloud to install
- `nextcloud_download_url`: The URL to download the Nextcloud release from
- `nextcloud_checksum_url`: The checksum of the download
- `nextcloud_signature_url`: The URL of the Nextcloud download's GPG signature
- `nextcloud_gpg_key_url`: The URL of the GPG key used to sign the download
- `nextcloud_tls_key`: The path to the TLS key file
- `nextcloud_tls_cert`: The path to the full TLS certificate chain file
- `nextcloud_redis_socket_path`: The path to the Redis socket
- `nextcloud_php_version`: The PHP version to install, defaults to 8.2
- `nextcloud_php_command`: The PHP command to use, defaults to `php`

## Example

Create an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault) to store sensitive values required by Nextcloud.

```sh
ansible-vault create nextcloud_secrets.yml
```

Edit the file to include the following secrets:

```yaml
---
nextcloud_mysql_password: '<MySQL password to set for Nextcloud user>'
nextcloud_instance_id: '<?>'
nextcloud_redis_password: '<Redis password to set>'
nextcloud_admin_user: '<initial Nextcloud admin user to create>'
nextcloud_admin_pass: '<password for initial Nextcloud admin user>'
```

Use the role:

```yaml
---
- name: Install Nextcloud
  ansible.builtin.include_role:
    name: brianreumere.software.nextcloud
  vars_files:
    - nextcloud_secrets.yml
  vars:
    nextcloud_hostname: cloud.example.net
```
