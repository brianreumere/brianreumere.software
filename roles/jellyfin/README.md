# jellyfin

## Overview

Installs [Jellyfin](https://jellyfin.org/). Optionally configures an [nginx reverse proxy](https://jellyfin.org/docs/general/networking/nginx).

## Variables

### Required

If `jellyfin_reverse_proxy` is `true`, the following variables for the nginx configuration are required:

- `jellyfin_private_ip`: The IP address of the Jellyfin server
- `jellyfin_hostname`: The full hostname of the Jellyfin server
- `jellyfin_tls_cert`: The path to the TLS certificate
- `jellyfin_tls_key`: The path to the TLS key


### Optional

- `jellyfin_reverse_proxy`: Whether an nginx reverse proxy is configured, defaults to `false`

## Example

```yaml
---
- name: Install Jellyfin
  ansible.builtin.include_role:
    name: brianreumere.software.jellyfin
```
