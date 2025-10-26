🔥 Firewall Automation (firewalld)

Deze sectie bevat twee playbooks die automatische configuratie van Firewalld demonstreren op zowel Ubuntu (Debian family) als AlmaLinux/Rocky (RedHat family).

🔹 playbooks/firewall.yml

Een basis-playbook dat Firewalld installeert en een HTTPS-regel toevoegt in de standaardzone.
Het toont de beginselen van servicebeheer via Ansible.

Taken:

Installeert Firewalld

Start en enabled de service

Staat HTTPS toe in de internal zone

Toont de eindstatus met firewall-cmd --list-all

Run:

ansible-playbook playbooks/firewall.yml

🔹 playbooks/firewall_smart.yml

Een geavanceerde variant die automatisch de actieve zone detecteert en zich aanpast per Linux-distro.

Belangrijkste functies:

Detecteert of de interface al in een Firewalld-zone zit

Gebruikt de juiste package manager (apt/dnf) afhankelijk van distro

Schakelt UFW uit op Ubuntu om conflicten te vermijden

Stelt dynamisch de active_zone variabele in

Staat HTTPS en controller-IP toe in de juiste zone

Idempotent (kan veilig opnieuw worden uitgevoerd)

Variabelen:

controller_ip: 192.168.56.14
iface: enp0s3


Run:

ansible-playbook playbooks/firewall_smart.yml


Verify op target:

sudo firewall-cmd --get-active-zones
sudo firewall-cmd --zone=public --list-all


Voorbeeld output:

public (active)
  interfaces: enp0s3
  sources: 192.168.56.14
  services: dhcpv6-client https ssh


💡 Tip: deze playbooks tonen het verschil tussen een statische declaratie (firewall.yml) en een dynamische, portable configuratie (firewall_smart.yml).
Ideaal om in sollicitaties te laten zien dat je Ansible structureel, veilig en distro-agnostisch inzet.

---


🧩 *Stack:* Ansible · Linux · YAML · Git · VirtualBox homelab  
🧑‍💻 Auteur: Walid Riani



## 🔥 Smart Firewall Playbook
Automates firewall configuration across multiple distros.

- Detects active zone automatically
- Installs and enables firewalld
- Allows HTTPS and controller source IP
- Works on both Ubuntu and Alma/RHEL

Run:
```bash
ansible-playbook playbooks/firewall_smart.yml