# slskd

## Overview

Installs and configures [slskd](https://github.com/slskd/slskd) behind and nginx reverse proxy.

## Variables

### Required

- `slskd_hostname`: The hostname of the slskd web UI
- `slskd_tls_cert`: The path to the TLS certificate file
- `slskd_tls_key`: The path to the TLS key file

## Example

```yaml
---
- name: Install slskd
  ansible.builtin.include_role:
    name: brianreumere.software.slskd
  vars:
    slskd_hostname: slsk.example.net
    slskd_tls_key: "{{ tls_key_path }}"
    slskd_tls_cert: "{{ tls_fullchain_path }}"
    slskd_share_directory: /mnt/Music
```
