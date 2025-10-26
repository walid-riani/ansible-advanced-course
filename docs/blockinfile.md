# 🧱 Blockinfile Automation (Add Web Content)

Automates insertion of a multi-line content block into an existing file using Ansible’s **`blockinfile`** module.  
This playbook targets `/var/www/html/index.html` on the `web1` node and ensures the file is properly owned by Apache.

---

## 🎯 Objective

- Add a content block **at the beginning** of the `/var/www/html/index.html` file.
- Ensure:
  - The block text is added only once (idempotent).
  - File owner and group are both `apache`.
  - File permissions are `0644`.

---

## ⚙️ Playbook

**Path:** `playbooks/index2.yml`

```yaml
---
- name: Add content block to index.html on web1
  hosts: web1
  become: true
  tasks:
    - name: Insert welcome message at the top of index.html
      ansible.builtin.blockinfile:
        path: /var/www/html/index.html
        insertbefore: BOF
        owner: apache
        group: apache
        mode: '0644'
        block: |
          Welcome to KodeKloud!
          This is Ansible Lab.