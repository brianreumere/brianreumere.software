# graphite-exporter

## Overview

Installs [Graphite Exporter](https://github.com/prometheus/graphite_exporter) on RHEL.

## Variables

### Required

- `graphite_exporter_download_url`: The URL to download Graphite Exporter from
- `graphite_exporter_checksum`: The checksum of the download
- `graphite_exporter_version`: The version of Graphite Exporter

### Optional

- `graphite_exporter_install_dir`: Defaults to `/var/lib/graphite-exporter`
