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

### when multiple

```yaml
---
- name: print msg only when multiple condition are meet
  remote_user: ubuntu
  gather_facts: yes
  hosts: all
  tasks:
    - name: print msg based on facts
      debug:
        msg: "when condition are meet"
      when:
        - ansible_distribution == "Ubuntu"
        - ansible_memfree_mb > 1000
```

### when complex

```yaml
---
- name: print msg only when multiple complex conditions are meet
  remote_user: ubuntu
  gather_facts: yes
  hosts: all
  tasks:
    - name: print msg based on facts
      debug:
        msg: "complex when conditions are meet"
      when: >
        ( ansible_distribution == "Ubuntu" and
          ansible_memfree_mb > 1000 )
        or
        ( ansible_distribution == "Ubuntu" and
          ansible_memfree_mb > 100 )
```

### handlers

Sometimes you want a task to run only when a change is made on a machine. For example, you may want to restart a service if a task updates the configuration of that service, but not if the configuration is unchanged.

{% hint style="info" %}
Handlers are tasks that only run when notified.

`notify: [handler name]`
{% endhint %}

* same yaml lvl as `tasks`

{% hint style="warning" %}
* will only run if all tasks are sucesfull, U can omit this by `ignore_errors: yes` on `hosts:` lvl
* will run only on change task, notify will by activated only for changed tasks
{% endhint %}

```yaml
---
- name: run handler only when copy file changed
  remote_user: ubuntu
  gather_facts: yes
  hosts: all
  tasks:
    - name: copy file
      copy:
        src: /tmp/test
        dest: /tmp/test
      notify:
        - print_msg

  handlers:
    - name: print_msg
      debug:
        msg: "Copy with chage, msb by handler"
```

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
* you can use `smart` gathering facts

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

## ad hoc cheat sheet

```bash
ansible all -m setup -a "filter=ansible_distribution" -u ubuntu
```

