# 🧠 Filecontent Automation (Find + Synchronize)

Automates discovery and transfer of large or old files between directories.  
Designed for cross-distro environments (Ubuntu, AlmaLinux, Rocky).

---

## 🎯 Objective

Recursively find files in `/opt/data` that:
- are **older than 2 minutes** (`age: +2m`)
- are **equal or greater than 1 MB** (`size: +1m`)

Then copy (sync) them to `/opt`, preserving:
- file permissions  
- timestamps  
- owner/group metadata  

---

## ⚙️ Playbook

**Path:** `playbooks/find_sync.yml`

```yaml
- name: Find & rsync files from /opt/data to /opt
  hosts: web1
  become: true
  vars:
    search_dir: /opt/data
    dest_dir: /opt

  tasks:
    - name: Ensure destination directory exists
      ansible.builtin.file:
        path: "{{ dest_dir }}"
        state: directory
        mode: '0755'

    - name: Ensure rsync is installed
      ansible.builtin.package:
        name: rsync
        state: present

    - name: Find eligible files
      ansible.builtin.find:
        paths: "{{ search_dir }}"
        recurse: true
        file_type: file
        age: +2m
        size: +1m
      register: found

    - name: Rsync matched files
      ansible.posix.synchronize:
        src: "{{ item.path }}"
        dest: "{{ dest_dir }}/"
        archive: true
        compress: true
        rsync_opts:
          - "--checksum"
          - "--relative"
      loop: "{{ found.files }}"
      when: found.matched | default(0) > 0
      delegate_to: "{{ inventory_hostname }}"