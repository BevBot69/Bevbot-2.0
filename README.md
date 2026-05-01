# Bevbot 2.0

Ansible playbook to provision Bevbot 2.0 running [uCore](https://github.com/ublue-os/ucore).

**Services:** Jellyfin · Ollama  
**Containers:** Podman Compose  
**SSH:** Tailscale  
**Media:** NAS via SMB  

## Structure

```
bevbot2/
├── ansible.cfg
├── inventory.ini
├── playbook.yml
├── secrets.env          # 🔐 Encrypt before committing
├── tasks/
│   └── smb.yml
└── podman/
    └── compose.yml
```

## Setup

```bash
# Install dependencies
pip install ansible
ansible-galaxy collection install ansible.posix containers.podman

# Set your NAS details in playbook.yml vars block
# Fill in secrets.env, then encrypt it
ansible-vault encrypt secrets.env

# Enable Tailscale SSH on Bevbot 2.0 (one-time, via console)
sudo tailscale up --ssh

# Run
ansible-playbook playbook.yml
```

## Selective runs

Tags aren't used here — re-run the full playbook, it's idempotent.

## Secrets

`secrets.env` is encrypted with Ansible Vault and decrypted onto the server at runtime.  
The vault password is prompted automatically via `ask_vault_pass = True` in `ansible.cfg`.

> ⚠️ Only commit `secrets.env` **after** `ansible-vault encrypt secrets.env`
