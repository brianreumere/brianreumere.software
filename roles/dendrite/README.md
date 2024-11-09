# dendrite

## Overview

Installs [Dendrite](https://matrix-org.github.io/dendrite/) on OpenBSD. Only configures Dendrite to listen on localhost. You should configure a reverse proxy with TLS in front of it to expose it to the internet, and to serve the [well-known delegation files](https://matrix-org.github.io/dendrite/installation/domainname#well-known-delegation). For an example of this with relayd and httpd, see the [brianreumere.server.www role](https://galaxy.ansible.com/ui/repo/published/brianreumere/server/content/role/www/).

Only supports closed registration with a registration shared secret. Assumes you want to use a bare domain (e.g., example.net) as your server name and a subdomain for Dendrite (e.g., matrix.example.net).

Optionally installs [maubot](https://github.com/maubot/maubot) (with encryption support) and configures it to use the same PostgreSQL server as Dendrite.

## Variables

### Required

- `dendrite_server_name`: The value to set for `server_name` in the Dendrite configuration, commonly the bare domain (also requires a DNS record pointing this name to your server)
- `dendrite_matrix_hostname`: The value to use for the matrix hostname, commonly somethig like `matrix.example.net` (also requires a DNS record pointing this name to your server)
- `dendrite_postgresql_password`: Password for the PostgreSQL user, should be set in an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault)
- `dendrite_registration_shared_secret`: Shared secret for registration, should be set in an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault)

### Optional

- `dendrite_postgresql_user`: PostgreSQL username, defaults to `dendrite`
- `dendrite_well_known_webroot`: Where the `/.well-known/matrix/client` and `/.well-known/matrix/server` files will be served from, defaults to `/var/www/htdocs/matrix`
- `dendrite_trusted_id_servers`: A list of servers that Dendrite will trust as identity servers to verify third party identifiers, defaults to:
```yaml
- matrix.org
- vector.im
```
- `dendrite_disable_federation`: Disable federation, defaults to `false`
- `dendrite_dns_cache_enabled`: DNS cache setting, defaults to `true`
- `dendrite_dns_cache_size`: DNS cache size, defaults to 4096
- `dendrite_install_maubot`: Optionally install [maubot](https://github.com/maubot/maubot), defaults to `false`
- `dendrite_maubot_postgresql_user`: PostgreSQL username for maubot, defaults to `maubot`
- `dendrite_maubot_postgresql_password`: Password for the maubot PostgreSQL user, required if `dendrite_install_maubot` is `true`
- `dendrite_maubot_shared_secret`: Shared secret to sign maubot access tokens, required if `dendrite_install_maubot` is `true`
- `dendrite_maubot_admin_username`: Username for the maubot admin user, required if `dendrite_install_maubot` is `true`
- `dendrite_maubot_admin_password_hash`: bcrypt hash of the desired password for the maubot admin user (can be generated with `bcrypt.hashpw(b'<password>', bcrypt.gensalt())` in Python), required if `dendrite_install_maubot` is `true`

## Example

Create an [Ansible vault file](https://docs.ansible.com/ansible/latest/vault_guide/vault_encrypting_content.html#encrypting-files-with-ansible-vault) to store the PostgreSQL user's password and the registration shared secret:

```sh
ansible-vault create dendrite_secrets.yml
```

Edit the file to include your PostgreSQL password:

```yaml
---
dendrite_postgresql_password: '<password>'
dendrite_registration_shared_secret: '<registration shared secret>'
```

Use the role.

```yaml
---
- name: Set up Dendrite
  ansible.builtin.include_role:
    name: brianreumere.software.dendrite
  vars_files:
    - dendrite_secrets.yml
  vars:
    dendrite_server_name: example.net
```

## Further setup

Use `dendrite-create-account -config /etc/dendrite.yaml -username <your_username> -admin` to create an admin account.

If you install maubot, you should run the following commands manually as root if you want to be able to use other `mbc` commands like `mbc upload` to upload plugins:

```sh
mkdir /root/.config
mbc login
```

Specify the maubot admin username and password, and `http://localhost:29316` for the server. You can then use `mbc auth` to get an access token and device ID for a bot user that you create (with `dendrite-create-account -config /etc/dendrite.yaml -username <bot_username>`).
