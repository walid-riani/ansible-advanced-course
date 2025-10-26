# 🧠 Ansible Advanced Course (DevOps Homelab)

Dit project bevat mijn hands-on labs van de **KodeKloud Advanced Ansible Course**.  
Het is opgezet als een real-world DevOps repository met playbooks, roles en documentatie.

---

## 📚 Documentatie
| Onderdeel | Beschrijving |
|------------|--------------|
| [Setup](docs/setup.md) | Inventory, ansible.cfg en ping-test |
| [Firewall](docs/firewall.md) | Firewalld automatisering (smart vs static) |
| [File Content](docs/filecontent.md) | Config filebeheer (blockinfile, lineinfile) — *coming soon* |

---

| 🌐 **HTTPD Port** | Switch Apache httpd to port 8080 (safe replace + SELinux) | docs/httpd_port.md |


| 🧱 **Blockinfile** | Insert a web banner block in `/var/www/html/index.html` using Ansible’s blockinfile | [docs/blockinfile.md](docs/blockinfile.md) |

## 📚 Documentatie

- 🔥 **Firewall** – [docs/firewall.md](docs/firewall.md)  
- 📁 **Filecontent (Find + Synchronize)** – [docs/filecontent.md](docs/filecontent.md)

## ▶️ Snel starten

```bash
# alle playbooks draaien met eigen inventory/config
ansible-playbook playbooks/find_sync.yml -l web1

---

*(Let op: bij GitHub markdown geen lege codefence in codefence—bovenstaand blok in één keer kopiëren.)*

## 3) Commit & push README
```bash
git add README.md
git commit -m "docs: link Filecontent docs in README and add quickstart"
git push origin main