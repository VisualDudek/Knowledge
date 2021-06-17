# Ansible

## sample dir layout

{% embed url="https://docs.ansible.com/ansible/latest/user\_guide/sample\_setup.html\#sample-directory-layout" %}

## ansible.cfg

{% hint style="info" %}
copy from `/etc/ansible/ansible.cfg` and edit for best usage
{% endhint %}

```text
[defaults]

inventory = ./inventory/inventory.yml
```

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

## templates

* place all templats into `./templates/` sub directory
* templates have `.j2` extension

```yaml
---
- name: crete file from template
  hosts: all
  remote_user: ubuntu
  gather_facts: yes
  vars:
    foo: "Marcin"
    bar: "test"
  tasks:
    - name: use template to copy file, can use for config creatation
      template:
        src: test_template.j2
        dest: /tmp/test_template.conf
```

```yaml
# ./tempates/test_template.j2
foo={{ foo }}
bar={{ bar }}
some line of config
#my IP address={{ ansible_facts.default_ipv4.address }}
```

How to create hosts file based on inventory hosts

```yaml
---
- name: update /tmp/hosts file dynamically
  hosts: all
  remote_user: ubuntu
  tasks:
    - name: update /tmp/hosts
      template:
        src: hosts.j2
        dest: /tmp/hosts
```

```text
# ./templates/hosts.j2
{% for host in groups['all'] %}
{{ hostvars[host]['ansible_facts']['default_ipv4']['address'] }} {{ hostvars[host]['ansible_facts']['fqdn'] }} {{ hostvars[host]['ansible_facts']['hostname'] }}
{% endfor %}
```

## vars

* if vars is firs item it need to be in double quotes
* always starts with letter
* keep vars inside `host_vars/<servername>` or `group_vars/<groupname>` files
* u can provied file with vars in playbook using `vars_files`
* can set var from cli: `ansible-playbook name.yml -e "key=value"`
* can privde file with vars from cli: `ansible-playbook name.yml -e "@user.lst"`
* lists are defined by hyphens

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

```yaml
# import file with vars using cli:
# ansible-playbook name.yml -e "@user.lst"
---
- host: local
  vars:
    userFile: /home/user/vars/list
  tasks:
  - name: create file
    file:
      state: touch
      path: "{{ userFile }}"
  - name: list users
    lineinfile:
      path: "{{ userFile }}"
      line: "{{ item }}"
    with_items:
      - "{{ staff }}"
      - "{{ faculty }}"
      - "{{ other }}"
      
# user.list
staff:
  - joe
  - john
  - bob
faculty:
  - matt
  - alex
other:
  - will
```

### dictionary vars

```yaml
employee:
    name: bob
    id: 42
    
# two formats to access dictionary
employee['name'] 
employee.name # first is safer due to python collison
```

### magic vars

```yaml
{{ hostvars['node1']['ansible_distribution'] }}
{{ groups['webservers'] }}
```

### Jinja2 filters

```yaml
{{ groups['webservers'] | join(' ') }}
```

{% embed url="https://jinja.palletsprojects.com/en/3.0.x/templates/\#filters" %}

## ansible\_facts

* you can disable gathering of facts `gather_facts: no`
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

{% hint style="danger" %}
What is this syntax: `msg: >` and how it diff to `|`
{% endhint %}

### facts.d

Create in `etc/ansible/facts.d/[name].fact` will be stored in `ansible_local` var. Facts files may be INI, JSON or and executable that returns JSON.

{% hint style="info" %}
executable lolcal facts seems cool, U can write pyhon script to retur JSON with local vars
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

## roles

By default, Ansible looks for roles in a dir called `roles/`

{% hint style="danger" %}
If U do not want that vars can be overriten put them into `vars` not in to `defaults`
{% endhint %}

```yaml
# motd-role.yml 
---
- name: use motd role playbook
  hosts: ansible2
  
  roles:
    - role: motd
      system_manager: bob@example #Override defaults vars in role
```

## Ansible Galaxy

* `ansible-galaxy search nginx`
* `ansible-galaxy install [role]`
* `ansible-galaxy init [role-name]`

{% hint style="info" %}
Galxy-init create nice dir structure
{% endhint %}

{% embed url="https://galaxy.ansible.com/linux-system-roles" caption="linux-system-roles" %}

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

### manipulate files

```yaml
---
- name: Manipulate file
  remote_user: ubuntu
  hosts: all
  tasks:
    - name: copy file demo
      copy:
        src: /tmp/hosts
        dest: /tmp/
    - name: add some lines to /tmp/hosts
      blockinfile:
        path: /tmp/hosts
        block: |
          555.555.555 host1
          555.555.777 host2
        state: present
    - name: verify file checksum
      stat:
        path: /tmp/hosts
        checksum_algorithm: md5
      register: result
    - debug:
        msg: "The checksum of /tmp/hosts is {{ result.stat.checksum }}"
    - name: fetch a file
      fetch:
        src: /tmp/hosts
        dest: /tmp/
```

### create users

```yaml
---
- name: create some users
  hosts: all
  become: yes
  #  vars_files:
  #    - vars/users
  #    - vars/groups
  remote_user: ubuntu
  tasks:
    - name: add groups
      group:
        name: "{{ item.groupname }}"
      loop: "{{ usergroups }}"
    - name: add users
      user:
        name: "{{ item.username }}"
        groups: "{{ item.groups }}"
      loop: "{{ users }}"
    - name: add SSH public keys
      authorized_key:
        user: "{{ item.username }}"
        key: "{{ lookup('file', 'files/' + item.username + '/id_rsa.pub') }}"
        state: present
      loop: "{{ users }}"
    - name: add marcin group members to sudo
      copy:
        content: "%marcin ALL=(ALL) NOPASSWD: ALL"
        dest: /etc/sudoers.d/marcin
        mode: 0440
```

```text
./files
├── adam
│   ├── id_rsa
│   └── id_rsa.pub
└── marcin
    ├── id_rsa
    └── id_rsa.pub
    
./group_vars
└── all.yml

# ./group_vars/all.yml
---
users:
  - username: marcin
    groups: marcin
  - username: adam
    groups: marcin
usergroups:
  - groupname: marcin
```

## ad hoc cheat sheet

```bash
ansible all -m setup -a "filter=ansible_distribution" -u ubuntu
```

## miscellaneous

### safely limiting to one server

{% embed url="https://stackoverflow.com/questions/18195142/safely-limiting-ansible-playbooks-to-a-single-machine" %}

