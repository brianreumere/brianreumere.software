# nginx

## Overview

Installs and configures nginx with no sites enabled.

## Variables

### Required

There are no required variables.

### Optional

- `deluge_hostname`: The hostname of the Deluge web UI
- `deluge_tls_cert`: The path to the TLS certificate file
- `deluge_tls_key`: The path to the TLS key file

- `nginx_delete_default_site`: Whether to delete the default site, defaults to `true`
- `nginx_enable_and_start`: Whether to enable and start the nginx service, defaults to `true`
- `nginx_worker_connections`: Worker connections, defaults to `768`
- `nginx_ssl_protocols`: SSL protocols, defaults to `TLSv1.3`
- `nginx_hsts`: Whether to enable HSTS, defaults to `true`
- `nginx_deny_frames`: Whether to set `X-Frame-Options` to deny, defaults to `true`
- `nginx_access_log_path`: Defaults to `/var/log/nginx/access.log`
- `nginx_error_log_path`: Defaults to `/var/log/nginx/error.log`
- `nginx_gzip`: Defaults to `off`

## Example

```yaml
---
- name: Install nginx
  ansible.builtin.include_role:
    name: brianreumere.software.nginx
```
