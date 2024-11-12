# acme_sh

## Overview

Installs [acme.sh](https://github.com/acmesh-official/acme.sh) and optionally issues and installs a certificate.

## Variables

### Required

- `acme_sh_email`: The email address to use when installing acme.sh
- `acme_sh_domains`: A list of domains to include on the issued certificate (the first domain specified will be the primary)
- `acme_sh_issue_method`: One of `webroot`, `standalone`, `standalone_ssl`, `apache`, `nginx`, `dns_standalone` or `dns_api`; if `webroot`, also requires `acme_sh_issue_webroot`; if `dns_api`, also requires `acme_sh_issue_dns_provider`
- `acme_sh_keyfile_path`: The path where the key will be installed, not required if `acme_sh_skip_install` is `true`
- `acme_sh_fullchain_path`: The path where the full certificate chain file will be installed, not required if `acme_sh_skip_install` is `true`
- `acme_sh_reload_cmd`: The reload command to run when the cert is installed, not required if `acme_sh_skip_install` is `true`

### Optional

- `acme_sh_install`: Whether or not to install acme.sh, defaults to `true`
- `acme_sh_version`: The version to install
- `acme_sh_download_url`: The URL to download the acme.sh release from
- `acme_sh_checksum`: The checksum of the downloaded release asset
- `acme_sh_home`: The home directory to install to, defaults to `/root/.acme.sh`
- `acme_sh_server`: The server to use to issue certificates, defaults to `letsencrypt`
- `acme_sh_issue_environment`: A dict suitable for passing to the `environment` argument of the certificate issue task (for example, to set environment variables to issue certs via a DNS API), defaults to `{}`
- `acme_sh_issue_webroot`: The webroot to use if `acme_sh_issue_method` is `webroot`, defaults to `/var/www/html`
- `acme_sh_issue_dns_provider`: The [DNS provider](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) to use if `acme_sh_issue_method` is `dns_api`, defaults to `dns_cf` (Cloudflare DNS)
- `acme_sh_force_issue`: Force the issue of the cert (includes `--force` in the issue command), defaults to `false`
- `acme_sh_skip_install`: Just issue a certificate and skip the install step, defaults to `false`

## Example

Create an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault) to store your DNS provider credentials (if `acme_sh_issue_method` is `dns_api`). This example will use [Cloudflare](https://github.com/acmesh-official/acme.sh/wiki/dnsapi#1-cloudflare-option):

```sh
ansible-vault create cloudflare_secrets.yml
```

Edit the file to include your Cloudflare token value:

```yaml
---
cloudflare_token: '<your Cloudflare token>'
cloudflare_zone_id: `<your Cloudflare zone ID>'
```

Use the role. This example issues a cert for `example.org` using the `dns_cf` (Cloudflare DNS) DNS API (including the required environment variables) and reloads the `httpd` service when the cert is installed.

```yaml
---
- name: Issue and install a certificate
  ansible.builtin.include_role:
    name: brianreumere.software.acme_sh
  vars_files:
    - cloudflare_secrets.yml
  vars:
    acme_sh_email: someone@example.net
    acme_sh_domains:
      - example.org
    acme_sh_issue_method: dns_api
    acme_sh_issue_dns_provider: dns_cf
    acme_sh_issue_environment:
      CF_Token: "{{ cloudflare_token }}"
      CF_Zone_ID: "{{ cloudflare_zone_id }}"
    acme_sh_keyfile_path: /etc/pki/tls/private/example.org.key
    acme_sh_fullchain_path: /etc/pki/tls/private/example.org.key
    acme_sh_reloadcmd: systemctl reload httpd
```
