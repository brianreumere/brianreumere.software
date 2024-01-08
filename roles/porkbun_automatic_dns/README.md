# porkbun_automatic_dns

## Overview

Installs [porkbun-automatic-dns](https://github.com/brianreumere/porkbun-automatic-dns) and creates a cron job to run it periodically.

## Variables

### Required

- `porkbun_automatic_dns_api_key`: Your Porkbun API key
- `porkbun_automatic_dns_secret_key`: Your Porkbun secret key
- `porkbun_automatic_dns_domain`: The domain to update DNS records for
- `porkbun_automatic_dns_record_names`: A list of DNS records to update

**`porkbun_automatic_dns_api_key` and `porkbun_automatic_dns_secret_key` should not be saved in plain text**. They should be encrypted using [Ansible vault](https://docs.ansible.com/ansible/latest/vault_guide/index.html) or stored in another secrets manager.

### Optional

- `porkbun_automatic_dns_version`: The version to install ([default](defaults/main.yml) should be up-to-date)
- `porkbun_automatic_dns_download_url`: The URL to download the release from ([default](defaults/main.yml) should be up-to-date)
- `porkbun_automatic_dns_checksum`: The checksum of the downloaded release asset ([default](defaults/main.yml) should be up-to-date)
- `porkbun_automatic_dns_install_dir`: The directory to copy the `pbad` script to, defaults to `/usr/local/bin`
- `porkbun_automatic_dns_ipv6`: Whether or not to update AAAA records instead of A records, defaults to `false`
- `porkbun_automatic_dns_ext_ip_method`: The method to get the external IP with, defaults to `porkbun` (Porkbun API), also accepts `stdin` and `extif`
- `porkbun_automatic_dns_ext_if`: The interface to get the external IP from (required if `porkbun_automatic_dns_ext_ip_method` is `extif`)
- `porkbun_automatic_dns_custom_keyfile`: The path to a custom keyfile that will be created, defaults to `false` (to fall back to `pbad`'s default)
- `porkbun_automatic_dns_ttl`: The TTL to set on the records, defaults to `false` (to fall back to `pbad`'s default)
- `porkbun_automatic_dns_cron_user`: The user whose crontab `pbad` will run from, defaults to `SUDO_USER` or `DOAS_USER` from the Ansible environment
- `porkbun_automatic_dns_cron_minute`: The minute field of the cron entry, defaults to `*/15` (every 15 minutes)
- `porkbun_automatic_dns_cron_hour`: The hour field of the cron entry, defaults to `*`
- `porkbun_automatic_dns_cron_day_of_month`: The day of month field of the cron entry, defaults to `*`
- `porkbun_automatic_dns_cron_month`: The month field of the cron entry, defaults to `*`
- `porkbun_automatic_dns_cron_day_of_week`: The day of week field of the cron entry, defaults to `*`

## Example

Create an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault) to store your Porkbun API key and secret key:

```sh
ansible-vault create porkbun_secrets.yml
```

Edit the file to include your API key and secret key values:

```yaml
---
porkbun_automatic_dns_api_key: '<your API key>'
porkbun_automatic_dns_secret_key: '<your secret key>'
```

Use the role. This example configures `pbad` to use the network interface `eth0` to determine the external IP address, and schedules it to run every 5 minutes instead of the default of every 15 minutes:

```yaml
---
- name: Set up porkbun-automatic-dns
  ansible.builtin.include_role:
    name: brianreumere.software.porkbun_automatic_dns
  vars_files:
    - porkbun_secrets.yml
  vars:
    porkbun_automatic_dns_domain: example.org
    porkbun_automatic_dns_record_names:
      - '@'
      - www
    porkbun_automatic_dns_ext_ip_method: extif
    porkbun_automatic_dns_ext_if: eth0
    porkbun_automatic_dns_cron_minute: '*/5'
```
