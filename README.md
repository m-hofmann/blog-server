# blog-server Setup Scripts

Ansible scripts for creating and setting up a simple web server

Secrets are managed using Ansible Vault

## Creation of a Cloud Server

- Creates a new cloud server
- Uses access token from vault
- Uses SSH key authentication by providing `~/.ssh/id_rsa.pub` to the Hetzner console

```bash
ansible-playbook create.yaml --vault-password-file secrets/vault.password
```

## Bootstrapping the server

- Installs basic tools (vim, tmux, htop)
- Set up basic hardening (fail2ban)
- Installs Nginx and configures it
- Setups up certbot and provides a Let's Encrypt certificate
  - Variables `domain_name` and `letsencrypt_mail` are taken from host_vars
  - mail address is encrypted using ansible vault

```
ansible-playbook bootstrap.yaml --vault-password-file secrets/vault.password -i inventories/hosts.hcloud.yaml
```

