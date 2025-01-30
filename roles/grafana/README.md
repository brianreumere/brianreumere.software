# grafana

## Overview

Installs [Grafana](https://grafana.com/) OSS on RHEL behind an nginx reverse proxy.

## Variables

### Required

- `grafana_hostname`: The hostname where Grafana will be available
- `grafana_tls_cert`: Path to the TLS certificate to use
- `grafana_tls_key`: Path to the TLS key file to use

Use the role:

```yaml
---
- name: Install Grafana
  ansible.builtin.include_role:
    name: brianreumere.software.grafana
  vars:
    grafana_hostname: grafana.example.net
```
