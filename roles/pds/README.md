# dendrite

## Overview

Installs an [atproto PDS](https://github.com/bluesky-social/atproto/tree/main/packages/pds) server on OpenBSD.

## Variables

### Required

- `pds_hostname`: PDS hostname
- `pds_service_name`: PDS service name, something like "Personal PDS"
- `pds_contact_email_address`: PDS admin contact email
- `pds_email_from_address`: Address to send PDS email from

The following variables should be set in an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault).

- `pds_jwt_secret`: PDS JWT secret, generated with `openssl rand --hex 16`
- `pds_admin_password`: PDS admin password, should be a random value
- `pds_plc_rotation_key`: PDS PLC rotation key, generated with `openssl ecparam --name secp256k1 --genkey --noout --outform DER | tail --bytes=+8 | head --bytes=32 | xxd --plain --cols 32`
- `pds_blobstore_s3_access_key_id`: S3 access key ID
- `pds_blobstore_s3_secret_access_key`: S3 secret access key

### Optional

- `pds_port`: Port to listen on, defaults to `2583`
- `pds_accepting_repo_imports`: Whether the PDS is accepting imports, defaults to `false`
- `pds_blob_upload_limit`: PDS blob upload limit, defaults to `5242880` (5 MB)
- `pds_data_directory`: PDS data directory, defaults to `/var/pds/data`
- `pds_blobstore_s3_bucket`: Blobstore bucket name
- `pds_blobstore_s3_region`: Blobstore region
- `pds_blobstore_s3_endpoint`: Blobstore S3 endpoint
- `pds_blobstore_s3_force_path_style`: Whether to force S3 path style, defaults to `false`
- `pds_did_plc_url`: DID PLC URL, defaults to `https://plc.directory`
- `pds_crawlers`: PDS relays, defaults to `https://bsky.network`
- `pds_invite_required`: Whether invite codes are required, defaults to `true`
- `pds_email_smtp_url`: SMTP address for email, defaults to `smtp://127.0.0.1:25`
- `pds_rate_limits_enabled`: Whether rate limits are enabled, defaults to `false- `pds_rate_limit_bypass_key`: Rate limit key, required when `pds_rate_limits_enabled` is `true`
- `pds_rate_limit_bypass_ips`: IPs that are allowed to bypass rate limits, required when `pds_rate_limits_enabled is `true`

## Example

Create an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault).

```sh
ansible-vault create pds_secrets.yml
```

Edit the file to include the required variables:

```yaml
---
pds_jwt_secret: <PDS JWT secret>
pds_admin_password: <PDS admin password>
pds_plc_rotation_key: <PDS PLC rotation key>
pds_blobstore_s3_access_key_id: <access key ID>
pds_blobstore_s3_secret_access_key: <secret access key>
```

Use the role.

```yaml
---
- name: Set up PDS
  ansible.builtin.include_role:
    name: brianreumere.software.pds
  vars_files:
    - pds_secrets.yml
  vars:
    pds_hostname: pds.example.net
```

## Further setup

TBD
