# deluge

## Overview

Installs and configures the [Deluge Web UI](https://deluge.readthedocs.io/en/latest/reference/web.html) behind an nginx reverse proxy.

## Variables

### Required

- `deluge_hostname`: The hostname of the Deluge web UI
- `deluge_tls_cert`: The path to the TLS certificate file
- `deluge_tls_key`: The path to the TLS key file

## Example

```yaml
---
- name: Install Deluge
  ansible.builtin.include_role:
    name: brianreumere.software.deluge
  vars:
    deluge_hostname: delugeweb.example.net
    deluge_tls_cert: /etc/ssl/certs/delugeweb.example.net.key
    deluge_tls_key: /etc/ssl/private/delugeweb.example.net.key
```
