# prometheus

## Overview

Installs [Prometheus](https://prometheus.io) on RHEL. Only supports static targets.

## Variables

### Required

- `prometheus_download_url`: The URL to download Prometheus from
- `prometheus_checksum`: The checksum of the download
- `prometheus_version`: The version of Prometheus
- `prometheus_targets`: A list of hosts to scrape for metrics

### Optional

- `prometheus_retention_time`: Defaults to `1y`

Use the role:

```yaml
---
- name: Install Prometheus
  ansible.builtin.include_role:
    name: brianreumere.software.prometheus
  vars:
    grafana_hostname: grafana.example.net
    grafana_tls_cert: /etc/pki/tls/certs/grafana.example.net.crt
    grafana_tls_key: /etc/pki/tls/private/grafana.example.net.key
```
