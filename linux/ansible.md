# Ansible

### vars

* if vars is firs item it need to be in double quotes

```yaml
---
- name: create a user using var
  hosts: all
  vars:
    user: lisa
  
  tasks:
    - name: create user {{ user }}
      user:
        name: "{{ user }}"
```

### YAML structure

`- name: txt` is optional U can start with `- debug:` The dash is only for arry notice, in this case array element of `tasks:` 

```yaml
# how U can use Array

- name: Find files
  ansible.builtin.find:
    paths:
      - '/mnt/a'
      - '/mnt/b'
    patterns: '*.plot'
  register: results
- name: Print results var
  debug:
    msg: "{{ results.matched }}"
    
### U can skip name
- debug:
    msg: "{{ results.matched }}"
```

### Playbooks Examples

```yaml
---
- hosts: web
  become: yes
  tasks:
    - name: install httpd
      yum: name=httpd state=latest
    - name: start and enable httpd
      service: name=httpd state=started enabled=yes
    - name: retrieve website from repo
      get_url: url=http://repo.example.com/website.tgz dest=/tmp/website.tgz
    - name: install website
      unarchive: remote_src=yes src=/tmp/website.tgz dest=/var/www/html/
```



