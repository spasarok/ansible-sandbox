# [Roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html)

Roles let you automatically load variables files, tasks, and handlers based on a known file structure.

```
---
- hosts: webservers
  roles:
     - common
     - webservers
```

## Location

* `roles/` directory relative to the playbook
* `/etc/ansible/roles`

## File Structure

* Choose which directories to use for a role
* Any used directories must have a `main.yml`

```
roles/
   common/
     defaults/
     files/
     handlers/
     meta/
     tasks/
     templates/
     vars/
```

* `defaults` - default variables for the role
* `files` - files that can be deployed by this role
* `handlers` - handlers, which may be used by this role or even anywhere outside this role
* `meta` - role metadata
* `tasks` - main list of tasks for this role
* `templates` - templates that can be deployed by this role
* `vars` - other variables role variables

## Duplication

By default, role only executes (even if defined multiple times).

```
---
- hosts: webservers
  roles:
    - foo
    - foo
```

Will execute multiple times if definitions have different parameters.

```
---
- hosts: webservers
  roles:
  - role: foo
    vars:
      message: "first"
  - { role: foo, vars: { message: "second" } }
```

Can force to execute for each definition by setting `allow_duplicates: true` in role `meta/main.yml`.

```
# roles/role_name/meta/main.yml
allow_duplicates: true
```

## Dependencies

Can pull other roles as dependencies

* Must use classic role definition style
* Dependencies always executed before the role that includes them
* Dependencies may be recursive
* Dependencies follow duplication rules

```
---
dependencies:
  - role: common
    vars:
      some_parameter: 3
  - role: apache
    vars:
      apache_port: 80
```

## Embedding Modules and Plugins

Can embed a module and plugin directories inside of role directory.

* Modules and plugins will be accessible to this role and every role that executes thereafter.

## Examples

Inline roles (specified with tasks)

```
---
- hosts: webservers
  tasks:
    - debug:
        msg: "before we run our role"
    - import_role:
        name: static_role
    - include_role:
        name: dynamic_role
    - debug:
        msg: "after we ran our role"
```

Specify path to role

```
---

- hosts: webservers
  roles:
    - role: '/path/to/my/roles/common'
```

Keywords (new syntax)

```
---

- hosts: webservers
  roles:
    - common
    - role: foo_app_instance
      vars:
         dir: '/opt/a'
         app_port: 5000
    - role: foo_app_instance
      vars:
         dir: '/opt/b'
         app_port: 5001
```

Keywords (old syntax)

```
- hosts: webservers
  tasks:
  - include_role:
      name: foo_app_instance
    vars:
      dir: '/opt/a'
      app_port: 5000
```

Conditionally import roles

```
---

- hosts: webservers
  tasks:
    - include_role:
        name: some_role
      when: "ansible_facts['os_family'] == 'RedHat'"
```

Assign tags to tasks inside role (new syntax)

```
- hosts: webservers
  tasks:
  - import_role:
      name: foo
    tags:
    - bar
    - baz
```

Assign tags to tasks inside a role (old syntax)

```
---

- hosts: webservers
  roles:
    - role: bar
      tags: ["foo"]
    # using YAML shorthand, this is equivalent to the above
    - { role: foo, tags: ["bar", "baz"] }
    ---
```

Import the role itself (tags not added to tasks inside the included role)

```
- hosts: webservers
  tasks:
    - include_role:
        name: bar
      tags:
        - foo
```
