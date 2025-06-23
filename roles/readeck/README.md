# shiori

## Overview

Installs [shiori](https://github.com/go-shiori/shiori) and the Apache web server to act as a reverse proxy.

## Variables

### Required

- `shiori_hostname`: The hostname where shiori will be accessible

### Optional

- `shiori_version`: The version of shiori to install
- `shiori_download_url`: The URl to download the release from
- `shiori_checksum`: The checksum of the release file
- `shiori_env_vars`: A dict of environment variables to configure in the shiori systemd service file, defaults to `{}`
- `shiori_tls_key`: The path to the SSL key file, defaults to `/etc/pki/tls/private/{{ shiori_hostname }}.key`
- `shiori_tls_cert`: The path to the SSL certificate, defaults to `/etc/pki/tls/certs/{{ shiori_hostname }}.key`

## Example

```yaml
---
- name: Install shiori
  ansible.builtin.include_role:
    name: brianreumere.software.shiori
  vars:
    shiori_hostname: shiori.example.net
```
