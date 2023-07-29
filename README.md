# blog-server Setup Scripts

Ansible scripts for creating and setting up a simple web server on Hetzner Cloud.

Secrets are managed using Ansible Vault.

Note that this repo was created without prior knowledge of Ansible and might
not adhere to best practices :wink:

## Creation of a Cloud Server

- Creates a new cloud server
- Uses access token from vault
- Uses SSH key authentication by providing `~/.ssh/id_rsa.pub` to the Hetzner console

Command to create the server:

```bash
ansible-playbook create.yaml --vault-password-file secrets/vault.password
```

## Bootstrapping the server

A dynamic Ansible inventory is used to retrieve matching servers from Hetzner.

- Installs basic tools (vim, tmux, htop)
- Configures Debian
  - Enables unattended-upgrades
  - Add `sftp-user` which can be used to deploy the static blog
- Set up basic hardening 
  - fail2ban
  - firewall
- Installs Nginx and configures it
  - Supports rate limit using fail2ban
- Installs certbot and provides a Let's Encrypt certificate
  - Variables `domain_name` and `letsencrypt_mail` are taken from host_vars
  - Mail address is encrypted using ansible vault

Command to run playbook:

```bash
ansible-playbook bootstrap.yaml --vault-password-file secrets/vault.password -i inventories/hosts.hcloud.yaml
```

