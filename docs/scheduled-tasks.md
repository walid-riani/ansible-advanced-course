# 🕐 Ansible – Scheduled Tasks (Systemd Timer)

## 🎯 Doel
Automatisch het script `/root/free.sh` uitvoeren om de vrije geheugenruimte te controleren, **elke 2 uur**, via een moderne **systemd timer**.

---

## 📘 Playbook
**Bestand:** `playbooks/free_timer.yml`

```yaml
---
- name: Schedule /root/free.sh with systemd timer (every 2h)
  hosts: node00
  become: true
  gather_facts: true

  vars:
    svc_name: "free-memory"
    script_path: "/root/free.sh"
    on_boot_sec: "5min"
    on_unit_active_sec: "2h"

  pre_tasks:
    - name: Assert script exists
      ansible.builtin.stat:
        path: "{{ script_path }}"
      register: script_stat

    - name: Fail when script is missing
      ansible.builtin.fail:
        msg: "Script {{ script_path }} does not exist on target host."
      when: not script_stat.stat.exists

    - name: Ensure script is executable
      ansible.builtin.file:
        path: "{{ script_path }}"
        mode: "0750"

  tasks:
    - name: Install systemd service unit
      ansible.builtin.copy:
        dest: "/etc/systemd/system/{{ svc_name }}.service"
        mode: "0644"
        content: |
          [Unit]
          Description=Run free memory check script

          [Service]
          Type=oneshot
          ExecStart=/usr/bin/env sh {{ script_path }}
          User=root

      notify: Reload systemd

    - name: Install systemd timer unit (every 2h)
      ansible.builtin.copy:
        dest: "/etc/systemd/system/{{ svc_name }}.timer"
        mode: "0644"
        content: |
          [Unit]
          Description=Run {{ svc_name }} every 2 hours

          [Timer]
          OnBootSec={{ on_boot_sec }}
          OnUnitActiveSec={{ on_unit_active_sec }}
          AccuracySec=1min
          Unit={{ svc_name }}.service

          [Install]
          WantedBy=timers.target

      notify: Reload systemd

    - name: Enable & start timer
      ansible.builtin.systemd:
        name: "{{ svc_name }}.timer"
        enabled: true
        state: started

  handlers:
    - name: Reload systemd
      ansible.builtin.systemd:
        daemon_reload: true