# [Variables](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html)

## Variable Definitions

* Inventory (hosts) file
* Playbook
* Included files and roles
* Facts (injected automatically, usually with `ansible_` prefix)

## Accessing Variables

Variables are referenced with Jinja2 notation.

```
- hosts: nodes
  vars:
    text: "hello world"
  tasks:
    - name: echo
      command: /bin/echo {{ text }}
```

Lines beginning with a variable must be quoted.

```
- hosts: servers
  vars:
    app_path: "{{ base_path }}/file_name.yml"
```

Complex variables can be accessed with yaml dictionary notation.

```
{{ ansible_facts["eth0"]["ipv4"]["address"] }}

{{ ansible_facts.eth0.ipv4.address }}
```

Can also access array elements stored in variables.

```
{{ foo[0] }}
```

## Reserved Variables

Common ones listed below (not all of them).

* `host_vars`
* `group_names`
* `groups`
* `environment`

(View link to see what these do.)

## Registering Variables

Can register result of a task as a variable for rest of playbook run.

```
- hosts: web_servers
  tasks:
    - shell: /usr/bin/foo
      register: foo_result
      ignore_errors: True
    - shell: /usr/bin/bar
      when: foo_result.rc == 5
```

## Variable Files

Can split variables into external files to separate sensitive information.

```
---
- hosts: all
  remote_user: root
  vars:
    text: hello
  vars_files:
    - /vars/external_vars.yml
  tasks:
  - name: echo external var
    command: /bin/echo {{ external_var }}
```

```
---
# /vars/external_vars.yml
external_var: hello world
```

## Command Line Variables

key=value format

```
ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"
```

JSON string format

```
ansible-playbook release.yml --extra-vars '{"version":"1.23.45","other_variable":"foo"}'
ansible-playbook arcade.yml --extra-vars '{"pacman":"mrs","ghosts":["inky","pinky","clyde","sue"]}'
```

YAML string format

```
ansible-playbook release.yml --extra-vars '
version: "1.23.45"
other_variable: foo'
```

```
ansible-playbook arcade.yml --extra-vars '
pacman: mrs
ghosts:
- inky
- pinky
- clyde
- sue'
```

From JSON or YAML file

```
ansible-playbook release.yml --extra-vars "@some_file.json"
```

Escaping quotes

```
ansible-playbook arcade.yml --extra-vars "{\"name\":\"Conan O\'Brien\"}"
ansible-playbook arcade.yml --extra-vars '{"name":"Conan O'\\\''Brien"}'
ansible-playbook script.yml --extra-vars "{\"dialog\":\"He said \\\"I just can\'t get enough of those single and double-quotes"\!"\\\"\"}"
```

## Precedence

Precedence from lowest to highest.

* Command line values (eg `-u user`)
* Role defaults
* Group var files
  * Inventory file or script group vars
  * Inventory `group_vars/all`
  * Playbook `group_vars/all`
  * inventory `group_vars/*`
  * playbook `group_vars/*`
* Host var files
  * Inventory file or script host vars
  * Inventory `host_vars/*`
  * Playbook `host_vars/*`
* Facts
  * Host facts/cached `set_facts`
* Play
  * Play vars
  * Play `vars_prompt`
  * Play `vars_files`
* Role files
  * Role vars (defined in `role/vars/main.yml`)
* Block vars (only for tasks in block)
* Task vars (only for the task)
* `include_vars`
* `set_facts`/registered vars
* Role (and `include_role`) params
* Include params
* Extra vars command line flag

## Scope

* Global
  * Config
  * Environment variables
  * Command line
* Play
  * Each play
  * Contained structures
  * Var entries (vars; vars_files; vars_prompt)
  * Role defaults
  * Vars
* Host
  * Variables directly associated to a host
  * ex. inventory
  * ex. include_vars
  * ex. facts
  * ex. registered task outputs
