### Disable contractor account (hardening)
Playbook: `playbooks/disable_contractor.yml`

- Lock + expire now
- Remove from sudo (drop-in + privileged groups)
- Kill active sessions
- Optional: archive home → `/var/archives/<user>-<timestamp>.tar.gz`
- Optional: disable `authorized_keys`

Run:
```bash
ansible-playbook playbooks/disable_contractor.yml -e "user_name=neymarsabin archive_home=true"