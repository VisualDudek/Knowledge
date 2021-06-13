# Ansible

## ansible.cfg

## TASK CONTROL

### when

```yaml
---
- name: print msg only when corond is running
  remote_user: ubuntu
  gather_facts: yes
  hosts: all
  tasks:
    - name: get the cron server status
      command: systemctl is-active cron
      ignore_errors: yes
      register: result
    - name: print msg based on crond status
      debug:
        msg: "Crond is running"
      when: result.rc == 0
```

`rc` stands for **return code**

## vars

* if vars is firs item it need to be in double quotes
* keep vars inside `host_vars/<servername>` or `group_vars/<groupname>` files
* can set var from cli: `ansible-playbook name.yml -e "key=value"`

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

## ansible\_facts

* you can disable gathering of facts

{% hint style="warning" %}
in `var:` U do not need double quote
{% endhint %}

```yaml
---
- name: show facts
  hosts: all
  tasks:
    - name: print facts
      debug:
        var: ansible_facts
    - name: be more specific about facts
      debug:
        msg: >
          This host uses IP address {{ andible.facts.default_ipv4.address }}
```

{% hint style="info" %}
What is this syntax: `msg: >` 
{% endhint %}

## ansible-vault

* run `ansible-playbook` with `--ask-vault-pass` flag.
* you can store passwd in file and restrict access than run playbook with:`--vault-password-file=filename`

```bash
# yaml file protected password
ansible-vault create secret.yml
```

```yaml
---
- name: create a user using var from vault
  hosts: all
  vars_files:
    - secret.yml
  tasks:
    - name: creating user
      user:
        name: "{{ username }}"
        password: "{{ pwhash }}"
```

## YAML structure

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

### include

```yaml
---
- host: lamp
  tasks:
    - name: include lamp tasks
      include: dir/book.yml
```

```yaml
# book.yml OLNY TASKS
---
  - name: install and start the servers
    yum:
      name:
        - "{{ ftp_package }}"
        - httpd
      state: latest
  - name: start ftp server
    service:
      name: "{{ ftp_package }}"
      state: started
      enabled: true
```

## Playbooks Examples

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



