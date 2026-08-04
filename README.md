# HomeLab-Private

Private backup store for homelab **data** that must survive node rebuilds.

Compose templates and runtime secrets live in the public `Kubernetes-Home-Lab` repo and GitHub Actions — not here.

## Layout

```
pocket-id/data/                          # Pocket ID DB, OIDC clients, uploads
pocket-id/pocket-id.env                  # Pocket ID ENCRYPTION_KEY + tunnel token backup
kubernetes/sealed-secrets-key-backup.yaml # Sealed-secrets master key (manual)
```

## Restore

**VPS (Pocket ID):** GitHub Actions → **VPS**, or:

```bash
cd Kubernetes-Home-Lab/vps/ansible
ansible-playbook -i inventory/hosts.ini playbooks/vps-restore.yml \
  -e github_app_token="$TOKEN" \
  -e pocket_id_encryption_key="$POCKET_ID_ENCRYPTION_KEY" \
  -e cloudflare_pocket_id_tunnel_token="$TUNNEL_TOKEN"
```

**Sealed-secrets key:** after cluster install, via `kubernetes/scripts/seal-secrets-key-backup.sh restore`.

## Backup

GitHub Actions → **Private Backup** on `Kubernetes-Home-Lab` (rsyncs `pocket-id/data/` only).

## Security

This repository is private. Do not fork publicly.
