# jellyfin

## Overview

Installs [Jellyfin](https://jellyfin.org/). Optionally configures an [nginx reverse proxy](https://jellyfin.org/docs/general/networking/nginx).

## Variables

### Required

If `jellyfin_reverse_proxy` is `true`, the following variables for the nginx configuration are required:

- `plex_private_ip`: The IP address of the Plex server
- `plex_port`: The port that Plex runs on, defaults to `32400`
- `plex_hostname`: The full hostname of the Plex server
- `plex_tls_cert`: The path to the TLS certificate
- `plex_tls_key`: The path to the TLS key


### Optional

- `plex_reverse_proxy`: Whether an nginx reverse proxy is configured (overwrites `/etc/nginx/nginx.conf`, not compatible with other sites running on the same server), defaults to `false`

## Example

```yaml
---
- name: Install Plex
  ansible.builtin.include_role:
    name: brianreumere.software.plex
```
