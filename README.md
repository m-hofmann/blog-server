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
- Installs Nginx

```
ansible-playbook bootstrap.yaml --vault-password-file secrets/vault.password -i inventories/hosts.hcloud.yaml
```

