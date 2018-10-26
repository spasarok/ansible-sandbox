# [Intro to Playbooks](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html)

* Basics
  * YAML
  * Plays map host to roles that perform tasks
  * Tasks make calls to modules
  * Handlers trigger events when notified that all tasks are complete
* Sequencing
  * Plays and tasks run in the order specified
  * Host order within a play defaults to inventory order unless otherwise specified
    * `inventory`
    * `reverse_inventory`
    * `sorted`
    * `reverse_sorted`
    * `shuffle`
* Commands
  * Run a playbook from control machine
    * `ansible-playbook {{playbook_yml}} -f 10`
  * Pull a playbook and run from host node
    * `ansible-pull`
* Tips and tricks
  * `ansible-playbook {{playbook_yml}} --syntax-check` to check syntax (includes files, roles, etc)
  * `ansible-playbook {{playbook_yml}} --verbose` to get detailed output
  * `ansible-playbook {{playbook_yml}} --list-hosts` to list hosts affected by a playbook
  * End of playbook execution summarizes node failures and "unreachable" attempts

## Examples

### Shell Commands

* Arguments are passed as a list, not key/value pairs
* Cares about getting an exit code of 0
  * `/usr/bin/somecommand || /bin/true` if command success returns something other than 0
  * `ignore_errors: True` if command success returns something other than 0

```
---
- hosts: web_servers
  remote_user: root
  tasks:
    - name: Run a shell command
      command: /bin/echo hello world

- hosts: api_servers
  remote_user: root
  tasks:
    - name: Run a shell command
      shell: /bin/echo hello world
```

### Variables

* Referenced with `{{ variable }}`

```
- hosts: web_servers
  vars:
    some_var: hello
    some_other_var: world
  remote_user: root
  tasks:
    - name: Run a shell command
      command: /bin/echo {{ some_var }} {{ some_other_var }}
```

### Modules

* Arguments are passed as key/value pairs
* `key: value` syntax is preferred to a list of `key=value`

```
---
# Preferred
- hosts: apache_servers
  remote_user: root
  tasks:
    - name: Run yum module
      yum:
        name: httpd
        state: latest

# Old    
- hosts: nginx_servers
  remote_user: root
  tasks:
    - name: Run yum module
      yum: name=nginx state=present
```

### Services

```
---
- hosts: apache_servers
  remote_user: root
  tasks:
    - name: Ensure service is running
      service:
        name: httpd
        state: started
```

### Templates

```
---
- hosts: apache_servers
  remote_user: root
  tasks:
    - name: Use a template to write a file
      template:
        src: /srv/httpd.j2
        dest: /etc/httpd.conf
```

### Handlers

* Trigger events when notified that all tasks are complete
* `notify` handlers run in the order they are defined, not the order listed in the `notify` statement
*
* Handler names and listen topics live in a global namespace.
* If two handler tasks have the same name, only one will run.

```
---
# Note that the "done" task is only triggered once, after all tasks are complete

- hosts: nodes
  remote_user: root
  handlers:
    - name: done
      command: /bin/echo done
    - name: goodbye
      command: /bin/echo goodbye
  tasks:
    - name: hello
      command: /bin/echo hello
      notify:
        - done
    - name: world
      command: /bin/echo world
      notify:
        - done
        - goodbye
```

Handlers can also listen.

```
- hosts: nodes
  remote_user: root
  handlers:
    - name: done
      command: /bin/echo done
      listen: print stuff
    - name: goodbye
      command: /bin/echo goodbye
      listen: print stuff
  tasks:
    - name: hello
      command: /bin/echo hello
      notify:
        - print stuff
    - name: world
      command: /bin/echo world
      notify:
        - print stuff
```

## Sequencing

1. `pre_tasks` defined in the play
1. Handlers triggered so far
1. Roles execute in order listed
  * Role dependencies defined in `meta/main.yml` run first
1. Tasks defined in the play
1. Handlers triggered so far
1. `post_tasks` defined in the play
1. Handlers triggered so far
