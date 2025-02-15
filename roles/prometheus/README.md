# prometheus

## Overview

Installs [Prometheus](https://prometheus.io) on RHEL. Only supports static targets.

## Variables

### Required

- `prometheus_download_url`: The URL to download Prometheus from
- `prometheus_checksum`: The checksum of the download
- `prometheus_version`: The version of Prometheus
- `prometheus_targets`: A list of hosts to scrape for metrics
- `prometheus_users`: A list of users and passwords for basic auth

### Optional

- `prometheus_install_dir`: Defaults to `/var/lib/prometheus`
- `prometheus_retention_time`: Defaults to `1y`
